---
layout: HubPage
hide_bc: true
title: Docs.microsoft.com Card Type Guide
description: This page serves to demonstrate the various card types in the docs.microsoft.com hub page design.
---
<div id="main" class="v2">
    <div class="container">
        <ul class="cardsY panelContent featuredContent">
            <li>
                <a href="#">
                    <div class="cardSize">
                        <div class="cardPadding">
                            <div class="card">
                                <div class="cardImageOuter">
                                    <div class="cardImage">
                                        <img data-hoverimage="/media/hubs/vs/setup-install.svg" src="/media/hubs/vs/setup-install.png" alt="" />
                                    </div>
                                </div>
                                <div class="cardText">
                                    <h3>Featured Content</h3>
                                    <p>Up to three cards at the top of the page. Containing &lt;ul&gt; uses the cardsY card class. Also apply panelContent and featuredContent classes.</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </a>
            </li>
            <li>
                <a href="#">
                    <div class="cardSize">
                        <div class="cardPadding">
                            <div class="card">
                                <div class="cardImageOuter">
                                    <div class="cardImage">
                                        <img data-hoverimage="/media/hubs/vs/getstarted.svg" src="/media/hubs/vs/getstarted.png" alt="" />
                                    </div>
                                </div>
                                <div class="cardText">
                                    <h3>Image requires PNG and SVG</h3>
                                    <p>The PNG file contains the default version of the image, referenced by the src attribute.</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </a>
            </li>
            <li>
                <a href="#">
                    <div class="cardSize">
                        <div class="cardPadding">
                            <div class="card">
                                <div class="cardImageOuter">
                                    <div class="cardImage">
                                        <img data-hoverimage="/media/hubs/vs/whatsnew.svg" src="/media/hubs/vs/whatsnew.png" alt="" />
                                    </div>
                                </div>
                                <div class="cardText">
                                    <h3>Use SVG for data-hoverimage attribute</h3>
                                    <p>The SVG file contains the default image as well as the version the user will see on hover, placed side by side.</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </a>
            </li>
        </ul>
    </div>
    <div class="container">
        <h1>Docs.microsoft.com Card Type Guide</h1>
        <p>Introductory text: Lorem ipsum dolor sit amet, consectetur adipiscing elit. In ultrices sollicitudin arcu, non consectetur massa vulputate sed. In nibh magna, efficitur vel luctus varius, cursus id neque. Nam rhoncus orci finibus lacus pharetra venenatis.</p>
        <ul class="pivots">
            <li>
                <a href="#cardtypes">Card Types</a>
                <ul id="cardtypes">
                    <li>
                        <div class="container intro">
                            <p>Pivot intro: Any pivot or panel can contain an intro; just use an &lt;li&gt; at the beginning of the list consisting of a &lt;div&gt; given the classes "container" and "intro".</p>
                        </div>
                    </li>
                    <li>
                        <a data-default="true" href="#cardtypes-1">cardsA</a>
                        <ul id="cardtypes-1" class="cardsA">
                            <li>
                                <div class="container intro">
                                    <p>Panel intro: Built just like the panel intro, but placed as the first &lt;li&gt; in the panel's &lt;ul&gt;.</p>
                                </div>
                            </li>
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img src="https://docs.microsoft.com/en-us/azure/media/index/virtualmachine.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>cardsA with static image</h3>
                                                <p>This one uses just the src attribute; src uses SVG or PNG.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a href="#cardtypes-2">cardsB</a>
                        <ul id="cardtypes-2" class="cardsB">
                            <li>
                                <div class="container intro">
                                    <p>Card style not currently in use</p>
                                </div>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a href="#cardtypes-3">cardsC</a>
                        <ul id="cardtypes-3" class="cardsC">
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage bgdAccent1">
                                                    <img src="https://docs.microsoft.com/en-us/azure/media/index/azure-arch-1.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>cardsC with static image and shaded background</h3>
                                                <p>To add a shaded background, apply the class "bgdAccent1" to the &lt;div&gt; containing the image; it should have the "cardImage" class already applied by the template.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a href="#cardtypes-4">cardsD</a>
                        <ul id="cardtypes-4" class="cardsD">
                            <li>
                                <div class="container intro">
                                    <p>Card style not currently in use</p>
                                </div>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a href="#cardtypes-5">cardsE</a>
                        <ul id="cardtypes-5" class="cardsE">
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img data-hoverimage="/media/hubs/windows/win_try-windows.svg" src="/media/hubs/windows/win_try-windows.png" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>cardsE with hover effect on image</h3>
                                                <p>The image uses both data-hoverimage and src attributes; data-hoverimage gets SVG image, src gets PNG version. Note: use empty alt attribute to indicate the image is decorative.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img src="https://docs.microsoft.com/en-us/azure/media/index/virtualmachine.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>cardsE with static image</h3>
                                                <p>This one uses just the src attribute; src uses SVG or PNG.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardText">
                                                <h3>cardsE without image</h3>
                                                <p>You can use cardsE without an image if you need to; simply remove the cardImageOuter div and its contents. However, if none of your cards have images, consider using cardsW or cardsZ.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a href="#cardtypes-6">cardsF</a>
                        <ul id="cardtypes-6" class="cardsF">
                            <li>
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img src="/media/logos/logo_TS.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <a href="">
                                                    <h3>cardsF with static image</h3>
                                                    <p>Hover effects don't work with cardsF. Use static images only; src uses SVG or PNG. Note: use empty alt attribute to indicate the image is decorative.</p>
                                                </a>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a href="#cardtypes-7">cardsFTitle</a>
                        <ul id="cardtypes-7" class="cardsFTitle">
                            <li>
                                <div class="container intro">
                                    <p>Panel intro: see cardsA for how to build. The cardsFTitle card type is unique: the only text that renders is the heading (&lt;h3&gt;). However, unlike cardsF, hover effects work; see examples below. If you don't have images, consider using another card type.</p>
                                </div>
                            </li>
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img data-hoverimage="/media/hubs/windows/win_academy.svg" src="/media/hubs/windows/win_academy.png" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>Lorem ipsum dolor sit amet</h3>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img src="/media/logos/logo_Fsharp.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>Lorem ipsum dolor sit amet</h3>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img src="/media/logos/logo_Fsharp.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>Lorem ipsum dolor sit amet</h3>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img src="/media/logos/logo_Fsharp.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>Lorem ipsum dolor sit amet</h3>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a href="#cardtypes-8">cardsG</a>
                        <ul id="cardtypes-8" class="cardsG">
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img src="/media/hubs/dotnet/net-docs-web-1.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>cardsG with static image</h3>
                                                <p>This one uses just the src attribute; src uses SVG or PNG.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a data-default="true" href="#cardtypes-9">cardsL</a>
                        <ul id="cardtypes-9" class="cardsL">
                            <li>
                                <div class="container intro">
                                    <h2 class="likeAnH1">Get started panel</h2>
                                    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus fermentum libero at orci finibus egestas. Duis viverra erat tincidunt efficitur dapibus.</p>
                                </div>
                            </li>
                            <li>
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardText">
                                                <h3>Featured landing pages</h3>
                                                <a class="barLink" href="/azure/virtual-machines/linux/index">Donec ullamcorper non justo at tempor.</a>
                                                <a class="barLink" href="/azure/virtual-machines/windows/index">Donec ornare augue ut fermentum porta.</a>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </li>
                            <li>
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardText">
                                                <h3>Quickstarts</h3>
                                                <div class="ico48Link">
                                                    <a href="http://microsoft.com">
                                                        <img src="/media/logos/logo_NET.svg" alt="">
                                                        <span>Tech label</span>
                                                    </a>
                                                </div>
                                                <div class="ico48Link">
                                                    <a href="http://microsoft.com">
                                                        <img src="/media/logos/logo_php.svg" alt="">
                                                        <span>Tech label</span>
                                                    </a>
                                                </div>
                                                <div class="ico48Link">
                                                    <a href="http://microsoft.com">
                                                        <img src="/media/logos/logo_nodejs.svg" alt="">
                                                        <span>Tech label</span>
                                                    </a>
                                                </div>
                                                <div class="ico48Link">
                                                    <a href="http://microsoft.com">
                                                        <img src="/media/logos/logo_java.svg" alt="">
                                                        <span>Tech label</span>
                                                    </a>
                                                </div>
                                                <div class="ico48Link">
                                                    <a href="http://microsoft.com">
                                                        <img src="/media/logos/logo_python.svg" alt="">
                                                        <span>Tech label</span>
                                                    </a>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </li>
                            <li>
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardText">
                                                <h3>More features</h3>
                                                <a class="barLink" href="/azure/sql-database/index">Another landing page</a>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </li>
                            <li>
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardText">
                                                <h3>More technologies</h3>
                                                <div class="ico48Link">
                                                    <a href="http://microsoft.com">
                                                        <img src="/media/logos/logo_java.svg" alt="">
                                                        <span>Tech label</span>
                                                    </a>
                                                </div>
                                                <div class="ico48Link">
                                                    <a href="http://microsoft.com">
                                                        <img src="/media/logos/logo_python.svg" alt="">
                                                        <span>Tech label</span>
                                                    </a>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a  href="#cardtypes-10">cardsW</a>
                        <ul id="cardtypes-10" class="cardsW">
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage">
                                                    <img src="https://docs.microsoft.com/en-us/azure/media/index/azure-arch-2.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>cardsW with static image</h3>
                                                <p>This card type is similar to cardsD, but with larger headings. Images use SVG or PNG.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardImageOuter">
                                                <div class="cardImage bgdAccent1">
                                                    <img src="https://docs.microsoft.com/en-us/azure/media/index/azure-arch-2.svg" alt="" />
                                                </div>
                                            </div>
                                            <div class="cardText">
                                                <h3>cardsW with static image and shaded background</h3>
                                                <p>To add a shaded background, apply the class "bgdAccent1" to the &lt;div&gt; containing the image; it should have the "cardImage" class already applied by the template.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardText">
                                                <h3>cardsW without image</h3>
                                                <p>You can use cardsW without an image if you need to; simply remove the cardImageOuter div and its contents. However, it's going to look inconsistent unless none of your cards have images.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                        </ul>
                    </li>
                    <li>
                        <a href="#cardtypes-11">cardsZ</a>
                        <ul id="cardtypes-11" class="cardsZ"> 
                            <li>
                                <a href="">
                                <div class="cardSize">
                                    <div class="cardPadding">
                                        <div class="card">
                                            <div class="cardText">
                                                <h3>cardsZ sample</h3>
                                                <p>The cardsZ type will not display images at all; use it for text-only card panels.</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </a>
                            </li>
                        </ul>
                    </li>
                </ul>
            </li>
        </ul>
    </div>
    <div class="container centered pageFooter">
        <h2>Page Footer: centered</h2>
        <ul class="links">
           <li>
                <a href="#">
                    Lorem ipsum
                </a>
            </li>
            <li>
                <a href="#">
                    dolor sit amet
                </a>
            </li>
            <li>
                <a href="#">
                    consectetur
                </a>
            </li>
            <li>
                <a href="#">
                    adipiscing elit
                </a>
            </li>
            <li>
                <a href="#">
                    In ultrices sollicitudin
                </a>
            </li>
        </ul>
    </div>
</div>