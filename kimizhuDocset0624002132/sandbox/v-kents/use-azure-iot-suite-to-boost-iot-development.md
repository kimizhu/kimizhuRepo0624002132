## Internet of Things

# Use Azure IoT Suite to Boost IoT Development

Dawid Borycki

---

An Internet of Things (IoT) solution comprises remote telemetry devices,
a Web portal, cloud storage and real-time processing. Such a complex
structure can make you reluctant to start IoT development. To make
things easier, the Microsoft Azure IoT Suite provides two preconfigured
solutions: remote monitoring and predictive maintenance. Here, I’ll tell
you how to create a remote monitoring solution, which will collect and
analyze data from a remote IoT device controlled by Windows 10 IoT Core.
This device, the Raspberry Pi, will acquire images from a USB camera.
Subsequently, the image brightness will be calculated on the IoT device
and then streamed to the cloud, where it will be stored, processed and
displayed (see **Figure 1**). Furthermore, the end user will not only be
able to see the information acquired with the remote device, but also
remotely control that device. You can find the complete source code
supporting this discussion at
[msdn.com/magazine/0617magcode](https://msdn.com/magazine/0617magcode).

**Figure 1 A Preconfigured Azure IoT Suite Solution Portal and the
Remote Universal Windows Platform App, Which Acquires and Processes the
Video Stream**

Remote Device
-------------

The basics of programming the Raspberry Pi with Windows 10 IoT Core was
already discussed in this magazine by Frank LaVigne
([msdn.com/magazine/mt694090](https://msdn.com/magazine/mt694090)) and
Bruno Sonnino
([msdn.com/magazine/mt808503](https://msdn.com/magazine/mt808503)).
LaVigne and Sonnino showed how to set up the development environment and
the IoT board, how to configure the IoT unit in your browser using the
Device Portal, and how to control GPIO ports with Windows 10 IoT Core.
Moreover, LaVigne mentioned in his article that IoT can be used to
program and control remote cameras. Here, I develop this thought and
explicitly show how to turn the Raspberry Pi into such a device.

To that end, I create the RemoteCamera Universal Windows Platform (UWP)
app using the Blank App (Universal Windows) Visual C\# project template,
and then set the target and minimum API versions to Windows 10
Anniversary Edition (10.0; Build 14393). I use this API version to
enable binding to methods, which allows me to directly wire methods of
the view model with events fired by visual controls:

```
&lt;Button x:Name="ButtonPreviewStart"
Content="Start preview"
Click="{x:Bind remoteCameraViewModel.PreviewStart}" /&gt;
```

Next, I declare the UI as shown in **Figure 1**. There are two tabs:
Camera capture and Cloud. The first contains controls for starting and
stopping camera preview, displaying the video stream and presenting
image brightness (a label and a progress bar). The second tab contains
two buttons, which you can use to connect a device to the cloud and to
register a device with the IoT portal. The Cloud tab also has a checkbox
that lets you enable streaming telemetry data.

Most of the logic associated with the UI is implemented within the
RemoteCameraViewModel class (see the ViewModels subfolder of the
RemoteCamera project). This class, apart from implementing a few
properties that are bound to the UI, handles the video acquisition,
image processing and cloud interaction. Those sub-functionalities are
implemented in separate classes: CameraCapture, ImageProcessor and
CloudHelper, respectively. I’ll discuss CameraCapture and ImageProcessor
shortly, while CloudHelper and related helper classes will be described
later in the context of the preconfigured Azure IoT solution.

Camera Capture
--------------

Camera capture (see CameraCapture.cs under the Helpers folder) is built
on top of two elements: Windows.Media.Capture.MediaCapture class and
Windows.UI.Xaml.Controls.CaptureElement. The former is used to acquire
video, while the latter displays the acquired video stream. I acquire
video with a webcam, so I have to declare the corresponding device
capability in the Package.appxmanifest.

To initialize the MediaCapture class, you invoke the InitializeAsync
method. Eventually, you can pass to that method an instance of the
MediaCaptureInitializationSettings class, which lets you specify capture
options. You can choose between streaming video or audio and select the
capture hardware. Here, I acquire only video from the default webcam
(see **Figure 2**).

**Figure 2 Camera Capture Initialization**
```
public MediaCapture { get; private set; } = new MediaCapture();
public bool IsInitialized { get; private set; } = false;
public async Task Initialize(CaptureElement captureElement)
{
if (!IsInitialized)
{
var settings = new MediaCaptureInitializationSettings()
{
StreamingCaptureMode = StreamingCaptureMode.Video
};

try

{

await MediaCapture.InitializeAsync(settings);

GetVideoProperties();

captureElement.Source = MediaCapture; IsInitialized = true;

}

catch (Exception ex)

{

Debug.WriteLine(ex.Message);

IsInitialized = false;

}

}

}
```

Next, using the Source property of the CaptureElement class instance, I
wire this object with the MediaCapture control to display video stream.
I also invoke helper method GetVideoProperties, which reads and then
stores the size of a video frame. I use this information later to get
the preview frame for processing. Finally, to actually start and stop
video acquisition, I invoke StartPreviewAsync and StopPreviewAsync of
the MediaCapture class. In the CameraCapture I wrapped those methods
with additional logic, verifying initialization and the preview state:

public async Task Start()

{

if (IsInitialized)

{

if (!IsPreviewActive)

{

await MediaCapture.StartPreviewAsync();

IsPreviewActive = true;

}

}

}

When you run the app, you can press the Start preview button, which
configures camera acquisition, and you’ll see the camera image after a
short while. Note that the RemoteCamera app is universal, so it can be
deployed without any changes to your development PC, a smartphone, a
tablet or to the Raspberry Pi. If you test the RemoteCamera app with
your Windows 10 PC, you need to ensure that apps are allowed to use your
camera. You configure this setting with the Settings app
(Privacy/Camera). To test the app with Raspberry Pi I use a low-budget
Microsoft Life Cam HD-3000. This is a USB webcam, so it’s automatically
detected by Windows 10 IoT Core after I connect it to one of the four
Raspberry Pi USB ports. You’ll find a full list of cameras compatible
with Windows 10 IoT Core at [bit.ly/2p1ZHGD](http://bit.ly/2p1ZHGD).
Once you connect your webcam to the Rasbperry Pi, it will appear under
the Devices tab of the Device Portal.

Image Processor
---------------

The ImageProcessor class calculates the brightness of the current frame
in the background. To perform the background operation, I create a
thread with the task-based asynchronous pattern, as shown in **Figure
3**.

**Figure 3 Calculating Brightness in the Background**

public event EventHandler&lt;ImageProcessorEventArgs&gt; ProcessingDone;

private void InitializeProcessingTask()

{

processingCancellationTokenSource = new CancellationTokenSource();

processingTask = new Task(async () =&gt;

{

while (!processingCancellationTokenSource.IsCancellationRequested)

{

if (IsActive)

{

var brightness = await GetBrightness();

ProcessingDone(this, new ImageProcessorEventArgs(brightness));

Task.Delay(delay).Wait();

}

}

}, processingCancellationTokenSource.Token);

}

Within the while loop, I determine image brightness, and then pass this
value to listeners of the ProcessingDone event. This event is fed with
an instance of the ImageProcessorEventArgs class, which has only one
public property, Brightness. The processing runs until the task receives
a cancellation signal. The key element of the image processing is the
GetBrightness method, shown in **Figure 4**.

**Figure 4 The GetBrightness Method**

private async Task&lt;byte&gt; GetBrightness()

{

var brightness = new byte();

if (cameraCapture.IsPreviewActive)

{

// Get current preview bitmap

var previewBitmap = await cameraCapture.GetPreviewBitmap();

// Get underlying pixel data

var pixelBuffer = GetPixelBuffer(previewBitmap);

// Process buffer to determine mean gray value (brightness)

brightness = CalculateMeanGrayValue(pixelBuffer);

}

return brightness;

}

I access the preview frame using GetPreviewBitmap of the CameraCapture
class instance. Internally, GetPreviewBitmap uses GetPreviewFrameAsync
of the MediaCapture class. There are two versions of
GetPreviewFrameAsync. The first, a parameterless method, returns an
instance of the VideoFrame class. In this case, you can get the actual
pixel data by reading the Direct3DSurface property. The second version
accepts an instance of the Video­Frame class and copies pixel data to
its SoftwareBitmap property. Here, I use the second option (see the
GetPreviewBitmap method of the CameraCapture class) and then access
pixel data through the CopyToBuffer method of the SoftwareBitmap class
instance shown in **Figure 5**.

**Figure 5 Accessing Pixel Data**

private byte\[\] GetPixelBuffer(SoftwareBitmap softwareBitmap)

{

// Ensure bitmap pixel format is Bgra8

if (softwareBitmap.BitmapPixelFormat != CameraCapture.BitmapPixelFormat)

{

SoftwareBitmap.Convert(softwareBitmap, CameraCapture.BitmapPixelFormat);

}

// Lock underlying bitmap buffer

var bitmapBuffer =
softwareBitmap.LockBuffer(BitmapBufferAccessMode.Read);

// Use plane description to determine bitmap height

// and stride (the actual buffer width)

var planeDescription = bitmapBuffer.GetPlaneDescription(0);

var pixelBuffer = new byte\[planeDescription.Height \*
planeDescription.Stride\];

// Copy pixel data to a buffer

softwareBitmap.CopyToBuffer(pixelBuffer.AsBuffer());

return pixelBuffer;

}

I first verify that the pixel format is BGRA8, which means the image is
represented with four 8-bit channels: three for the blue, green, and red
colors, and one for alpha or transparency. If the input bitmap has a
different pixel format, I perform an appropriate conversion. Next, I
copy pixel data to the byte array, whose size is determined by image
height multiplied by image stride
([bit.ly/2om8Ny9](http://bit.ly/2om8Ny9)). I read both values from an
instance of the BitmapPlaneDescription, which I obtain from the
BitmapBuffer object, returned by SoftwareBitmap.LockBuffer method.

Given the byte array with pixel data, all I need to do is calculate the
mean value of all the pixels. So, I iterate over the pixel buffer (see
**Figure 6**).

**Figure 6 Calculating the Mean Value of the Pixels**

private byte CalculateMeanGrayValue(byte\[\] pixelBuffer)

{

// Loop index increases by four since

// there are four channels (blue, green, red and alpha).

// Alpha is ignored for brightness calculation

const int step = 4;

double mean = 0.0;

for (uint i = 0; i &lt; pixelBuffer.Length; i += step)

{

mean += GetGrayscaleValue(pixelBuffer, i);

}

mean /= (pixelBuffer.Length / step);

return Convert.ToByte(mean);

}

Then, at each iteration, I convert a given pixel to the gray scale by
averaging values from each color channel:

private static byte GetGrayscaleValue(byte\[\] pixelBuffer, uint
startIndex)

{

var grayValue = (pixelBuffer\[startIndex\]

+ pixelBuffer\[startIndex + 1\]

+ pixelBuffer\[startIndex + 2\]) / 3.0;

return Convert.ToByte(grayValue);

}

Brightness is passed to the view through the ProcessingDone event. This
event is handled in the MainPage class (MainPage.xaml.cs), where I
display brightness in the label and a progress bar. Both controls are
bounded to the Brightness property of the RemoteCameraViewModel. Note
that ProcessingDone is raised from the background thread. Hence, I
modify the RemoteCameraViewModel.Brightness through the UI thread using
Dispatcher class, as shown in **Figure 7**.

**Figure 7 Modifying the RemoteCameraViewModel.Brightness Through the UI
Thread Using the Dispatcher Class**

private async void DisplayBrightness(byte brightness)

{

if (Dispatcher.HasThreadAccess)

{

remoteCameraViewModel.Brightness = brightness;

}

else

{

await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =&gt;

{

DisplayBrightness(brightness);

});

}

}

Provisioning the Remote Monitoring Solution
-------------------------------------------

To provision a solution you can use a dedicated portal at
[azureiotsuite.com](http://azureiotsuite.com). After logging in and
choosing your Azure subscription, you’ll be redirected to a page where
you press the “Create a new solution” rectangle. This will open a Web
site where you can choose one of two preconfigured solutions: predictive
maintenance or remote monitoring (see **Figure 8**). After choosing the
solution another form appears, which lets you set the solution name and
region for Azure resources. Here, I set the solution name and region to
RemoteCameraMonitoring and West US, respectively.

**Figure 8 Azure IoT Suite Preconfigured Solutions (Top) and Remote
Monitoring Solution Configuration (Bottom)**

When you provision the remote monitoring solution, the Azure IoT Suite
portal creates several Azure resources: IoT Hub, Stream Analytics jobs,
Storage and the App Service. The IoT Hub provides bidirectional
communication between the cloud and remote devices. Data streamed by
remote devices is transformed by a Stream Analytics job, which typically
filters out unnecessary data. Filtered data is stored or directed for
further analysis. Finally, the App Service is used to host the Web
portal.

Solution provisioning can also be done from the command line. To do so,
you can clone or download the solution source code from
[bit.ly/2osI4RW](http://bit.ly/2osI4RW), and then follow the instruction
at [bit.ly/2p7MPPc](http://bit.ly/2p7MPPc). You also have the option to
deploy a solution locally, as shown at
[bit.ly/2nEePNi](http://bit.ly/2nEePNi). In this case, no Azure App
Service is created because the solution portal runs on a local machine.
Such an approach can be particularly useful for development and
debugging or when you want to modify a preconfigured solution.

Once the provisioning is finished, you can launch the solution, and its
portal will appear in the default browser (refer back to **Figure 1**).
This portal has a few tabs. For the purpose of this arti­cle, I’ll limit
my attention to just two of them: dashboard and devices. Dashboard
displays the map with remote devices and the telemetry data they stream.
The Devices tab shows a list of remote devices, including their status,
capabilities and a description. By default, there are several emulated
devices. Let me explain how to register the new, non-emulated hardware.

Registering a Device 
---------------------

To register a device you use the solution portal, where you press the
Add a device hyperlink located in the bottom-left corner. You can then
choose between a simulated or a custom device. Pick the second option
and press the Add New button. Now you can define the Device ID. I set
this value to RemoteCamera. Afterward, the ADD A CUSTOM DEVICE form
displays device credentials (see **Figure 9**), which I’ll use later to
connect my IoT device to the IoT Hub.

**Figure 9 Summary of Device Registration**

Device Metadata and Communicating with the Cloud
------------------------------------------------

The device you add will appear in the devices list, and you can then
send device metadata or device information. Device information comprises
a JSON object that describes the remote device. This object tells the
cloud endpoint the capabilities of your device, and includes a hardware
description and the list of remote commands accepted by your device.
These commands can be sent by the end user to your device through the
IoT solution portal. In the RemoteCamera app, device info is represented
as the DeviceInfo class (in the AzureHelpers subfolder):

public class DeviceInfo

{

public bool IsSimulatedDevice { get; set; }

public string Version { get; set; }

public string ObjectType { get; set; }

public DeviceProperties DeviceProperties { get; set; }

public Command\[\] Commands { get; set; }

}

The first two properties of DeviceInfo specify whether a device is
simulated and define the version of the DeviceInfo object. As you’ll see
later, the third property, ObjectType, is set to a string constant,
DeviceInfo. This string is used by the cloud, specifically by the Azure
Stream Analytics job, to filter device info from telemetry data. Next,
DeviceProperties (see the AzureHelpers subfolder) holds a collection of
properties (like serial number, memory, platform, RAM) that describe
your device. Finally, the Commands property is a collection of remote
commands recognized by your device. You define each command by
specifying its name and a list of parameters, which are represented by
the Command and CommandParameter classes, respectively (see
AzureHelpers\\Command.cs).

To establish communication between your IoT device and the IoT Hub you
use the Microsoft.Azure.Devices.Client NuGet package. This package
provides the DeviceClient class, which you use to send and receive
messages to and from the cloud. You can create an instance of the
DeviceClient using either the Create or CreateFromConnectionString
static methods. Here, I use the first option (CloudHelper.cs in the
AzureHelpers folder):

public async Task Initialize()

{

if (!IsInitialized)

{

deviceClient = DeviceClient.Create( Configuration.Hostname,
Configuration.AuthenticationKey());

await deviceClient.OpenAsync();

IsInitialized = true;

BeginRemoteCommandHandling();

}

}

As you can see, the DeviceClient.Create method requires you to provide
the hostname of the IoT Hub and device credentials (the identifier and
the key). You obtain these values from the solution portal during device
provisioning (refer back to **Figure 9**). In the RemoteCamera app, I
store the hostname, device id and the key within the Configuration
static class:

public static class Configuration

{

public static string Hostname { get; } =
"&lt;iot-hub-name&gt;.azure-devices.net";

public static string DeviceId { get; } = "RemoteCamera";

public static string DeviceKey { get; } = "&lt;your\_key&gt;";

public static DeviceAuthenticationWithRegistrySymmetricKey
AuthenticationKey()

{

return new DeviceAuthenticationWithRegistrySymmetricKey(DeviceId,
DeviceKey);

}

}

Additionally, the Configuration class implements a static method
AuthenticationKey, which wraps device credentials into an instance of
the DeviceAuthenticationWithRegistrySymmetricKey class. I use this to
simplify creation of the DeviceClient class instance.

Once the connection is made, all you need to do is to send the
DeviceInfo, as shown in **Figure 10**.

**Figure 10 Sending Device Information**

public async Task SendDeviceInfo()

{

var deviceInfo = new DeviceInfo()

{

IsSimulatedDevice = false,

ObjectType = "DeviceInfo",

Version = "1.0",

DeviceProperties = new DeviceProperties(Configuration.DeviceId),

// Commands collection

Commands = new Command\[\]

{

CommandHelper.CreateCameraPreviewStatusCommand()

}

};

await SendMessage(deviceInfo);

}

The RemoteCamera app sends the device info, which describes the real
hardware, so IsSimulatedDevice property is set to false. As I mentioned,
ObjectType is set to DeviceInfo. Moreover, I set the Version property to
1.0. For DeviceProperties I use arbitrary values, which mostly consist
of static strings (see the SetDefaultValues method of the
DeviceProperties class). I also define one remote command, Update camera
preview, which enables remote control of the camera preview. This
command has one Boolean parameter, IsPreviewActive, which specifies
whether the camera preview should be started or stopped (see the
CommandHelper.cs file under AzureHelpers folder).

To actually send data to the cloud I implement the SendMessage method:

private async Task SendMessage(Object message)

{

var serializedMessage = MessageHelper.Serialize(message);

await deviceClient.SendEventAsync(serializedMessage);

}

Basically, you need to serialize your C\# object to a byte array that
contains JSON-formatted objects (see the MessageHelper static class from
AzureHelpers subfolder):

public static Message Serialize(object obj)

{

ArgumentCheck.IsNull(obj, "obj");

var jsonData = JsonConvert.SerializeObject(obj);

return new Message(Encoding.UTF8.GetBytes(jsonData));

}

Then you wrap the resulting array into the Message class, which you send
to the cloud with the SendEventAsync method of the DeviceClient class
instance. The Message class is the object, which supplements raw data (a
JSON object being transferred) with additional properties. These
properties are used to track messages being sent between devices and the
IoT Hub.

In the RemoteCamera app, establishing a connection with the cloud and
sending device info is triggered by two buttons located on the Cloud
tab: Connect and initialize and Send device info. The click event
handler of the first button is bound to the Connect method of the
RemoteCameraViewModel:

public async Task Connect()

{

await CloudHelper.Initialize();

IsConnected = true;

}

The click event handler of the second button is wired with the
SendDeviceInfo method of the CloudHelper class instance. This method was
discussed earlier.

Once you connect to the cloud, you can also start sending telemetry
data, much as you did when sending device information. That is, you can
use the SendMessage method to which you pass a telemetry object. Here,
this object, an instance of the Telemetry­Data class, has just a single
property, Brightness. The following shows a complete example of sending
telemetry data to the cloud, implemented within the SendBrightness
method of the CloudHelper class:

public async void SendBrightness(byte brightness)

{

if (IsInitialized)

{

// Construct TelemetryData

var telemetryData = new TelemetryData()

{

Brightness = brightness

};

// Serialize TelemetryData and send it to the cloud

await SendMessage(telemetryData);

}

}

SendBrightness is invoked right after obtaining the brightness,
calculated by the ImageProcessor. This is performed in the
ProcessingDone event handler:

private void ImageProcessor\_ProcessingDone(object sender,
ImageProcessorEventArgs e)

{

// Update display through dispatcher

DisplayBrightness(e.Brightness);

// Send telemetry

if (remoteCameraViewModel.IsTelemetryActive)

{

remoteCameraViewModel.CloudHelper.SendBrightness(e.Brightness);

}

}

So, if you now run the RemoteCamera app, start the preview and connect
to the cloud, you’ll see that brightness values are displayed in the
corresponding chart, as shown back in **Figure 1**. Note that, although
simulated devices of the preconfigured solution are dedicated to sending
temperature and humidity as telemetry data, you can also send other
values. Here, I’m sending brightness, which is automatically displayed
in the appropriate chart.

Handling Remote Commands 
-------------------------

The CloudHelper class also implements methods for handling remote
commands received from the cloud. Similarly, as with the case of
ImageProcessor, I handle commands in the background
(BeginRemoteCommandHandling of the CloudHelper class). Again, I use the
task-based asynchronous pattern:

private void BeginRemoteCommandHandling()

{

Task.Run(async () =&gt;

{

while (true)

{

var message = await deviceClient.ReceiveAsync();

if (message != null)

{

await HandleIncomingMessage(message);

}

}

});

}

This method is responsible for creating a task that continuously
analyzes messages received from the cloud endpoint. To receive a remote
message, you invoke the ReceiveAsync method of the DeviceClient class.
ReceiveAsync returns an instance of the Message class, which you use to
obtain a raw byte array containing JSON-formatted remote command data.
You then deserialize this array to the RemoteCommand object (see
RemoteCommand.cs in the AzureHelpers folder), which is implemented in
the MessageHelper class (the AzureHelpers subfolder):

public static RemoteCommand Deserialize(Message message)

{

ArgumentCheck.IsNull(message, "message");

var jsonData = Encoding.UTF8.GetString(message.GetBytes());

return JsonConvert.DeserializeObject&lt;RemoteCommand&gt;(

jsonData);

}

RemoteCommand has several properties, but you typically use just two:
name and parameters, which contain the command name and command
parameters. In the RemoteCamera app, I use these values to determine if
an expected command was received (see **Figure 11**). If so, I raise the
UpdateCameraPreviewCommandReceived event to pass that information to
listeners, then I inform the cloud that the command was accepted using
the CompleteAsync method of the DeviceClient class. If the command isn’t
recognized, I reject it using the RejectAsync method.

**Figure 11 Remote Messages Deserialization and Parsing**

private async Task HandleIncomingMessage(Message message)

{

try

{

// Deserialize message to remote command

var remoteCommand = MessageHelper.Deserialize(message);

// Parse command

ParseCommand(remoteCommand);

// Send confirmation to the cloud

await deviceClient.CompleteAsync(message);

}

catch (Exception ex)

{

Debug.WriteLine(ex.Message);

// Reject message, if it was not parsed correctly

await deviceClient.RejectAsync(message);

}

}

private void ParseCommand(RemoteCommand remoteCommand)

{

// Verify remote command name

if (string.Compare(remoteCommand.Name,

CommandHelper.CameraPreviewCommandName) == 0)

{

// Raise an event, when the valid command was received

UpdateCameraPreviewCommandReceived(this,

new UpdateCameraPreviewCommandEventArgs(

remoteCommand.Parameters.IsPreviewActive));

}

}

The UpdateCameraPreviewCommandReceived event is handled in the MainPage
class. Depending on the parameter value I obtain from the remote
command, I either stop or start the local camera preview. This operation
is again dispatched to the UI thread, as shown in **Figure 12**.

**Figure 12 Updating the Camera Preview**

private async void CloudHelper\_UpdateCameraPreviewCommandReceived(
object sender, UpdateCameraPreviewCommandEventArgs e)

{

if (Dispatcher.HasThreadAccess)

{

if (e.IsPreviewActive)

{

await remoteCameraViewModel.PreviewStart();

}

else

{

await remoteCameraViewModel.PreviewStop();

}

}

else

{

await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =&gt;

{

CloudHelper\_UpdateCameraPreviewCommandReceived(sender, e);

});

}

}

Sending Commands from the Cloud
-------------------------------

Finally, I show that the solution portal can be used to remotely control
your IoT device. For this, you use the Devices tab, where you need to
find and then click your device (see **Figure 13**).

**Figure 13 The IoT Portal Devices Tab Showing RemoteCamera Details**

This activates a device details pane, where you click the Commands
hyperlink. You’ll then see another form that allows you to choose and
send a remote command. The particular layout of this form depends on the
command you choose. Here, I have only one single-parameter command, so
there’s only one checkbox. If you clear this checkbox, and send the
command, RemoteCamera app will stop the preview. All commands you send,
along with their status, appear in the command history, as shown in
**Figure 14**.

**Figure 14 A Form for Sending Remote Commands to the IoT Device**

Wrapping Up
-----------

I showed how to set up the preconfigured remote monitoring Azure IoT
Suite solution. This solution collects and displays information about
images acquired with the webcam attached to the remote device, and also
lets you remotely control the IoT device. The companion source code can
run on any UWP device, so you don’t actually need to deploy it to the
Raspberry Pi. This article, along with the online documentation for the
remote monitoring solution and its source code, will help you to
jump-start comprehensive IoT development for practical remote
monitoring. As you see, with Windows 10 IoT Core, you can go far beyond
simple LED blinking.

**Dawid Borycki** is a software engineer and biomedical researcher,
author and conference speaker. He enjoys learning new technologies for
software experimenting and prototyping.

Thanks to the following Microsoft technical expert for reviewing this
article: *Rachel Appel*
