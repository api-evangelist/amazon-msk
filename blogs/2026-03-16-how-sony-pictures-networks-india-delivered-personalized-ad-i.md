---
title: "How Sony Pictures Networks India Delivered Personalized Ad Insertion at Scale on AWS"
url: "https://aws.amazon.com/blogs/media/how-sony-pictures-networks-india-delivered-personalized-ad-insertion-at-scale-on-aws/"
date: "Mon, 16 Mar 2026 14:04:47 +0000"
author: "Deepak Das"
feed_url: "https://aws.amazon.com/blogs/media/feed/"
---
<p>Sony Pictures Network India (<a href="https://www.sonypicturesnetworks.com/" rel="noopener" target="_blank">SPNI</a>) runs <a href="https://www.sonyliv.com/" rel="noopener" target="_blank">Sony LIV</a>, which is one of the leading popular over-the-top (OTT) video-streaming platform in India. The content offering includes original and acquired TV shows and films, alongside live sporting events.&nbsp;SPNI secured the media rights for the Asia Cricket Council (ACC) from 2025 to 2031, making the Asia Cup Cricket 2025 their first major tournament and a high-stakes debut. During this inaugural tournament, Sony LIV delivered personalized advertisements at scale to a peak of over 10 million concurrent viewers, serving billions of ads across 19 matches. Delivering personalized ad experiences during large-scale live sports streaming requires a highly resilient, latency optimized server-side ad insertion (SSAI) architecture and SPNI achieved it by leveraging <a href="https://aws.amazon.com/mediatailor/" rel="noopener" target="_blank">AWS Elemental MediaTailor</a> integrated with Google Ad Manager (GAM) as the Ad Decision Server (ADS) in a multi-CDN environment including <a href="https://aws.amazon.com/cloudfront/" rel="noopener" target="_blank">Amazon CloudFront</a>, maintaining viewer experience and ad delivery at scale throughout the tournament.</p> 
<p>The <a href="https://aws.amazon.com/premiumsupport/plans/unified-operations/media/" rel="noopener" target="_blank">AWS Unified Operations for Media</a> team supported SPNI throughout this journey, working alongside the media specialist team in India. The&nbsp;team worked as an extension of SPNI’s technical team, providing strategic architectural guidance, conducting workflow reviews, coordinating cross-team integrations with partners such as Akamai and GAM, and delivering real-time support during live streaming. This partnership ensured SPNI handled the unprecedented scale and complexity of live cricket streaming with personalized advertising.</p> 
<h3>Objective</h3> 
<p>Live sports broadcasts present one of the most complex streaming environments like high concurrency, unpredictable viewership spikes, and the need for flawless ad transitions. For this cricket tournament, SPNI wanted to</p> 
<ul> 
 <li>Deliver <strong>personalized ads in real time</strong> for every viewer based on device types, languages and other Ad campaign criteria.</li> 
 <li>Maintain <strong>broadcast-grade reliability</strong> and minimal-latency during live play.</li> 
 <li>Seamlessly integrate with their existing Google Ad Manager (GAM) for ad decisioning and impression reporting.</li> 
 <li>Handle multi-CDN distribution across India efficiently to ensure <strong>resilient global delivery.</strong></li> 
 <li>Deliver personalized ads across Android, iOS, and Connected TV (CTV) devices</li> 
</ul> 
<blockquote>
 <p>“Granular playback session level capabilities, combined with ad break type management and MediaTailor’s pre-fetch driven traffic shaping, enable just-in-time strategies that maximize concurrent personalized ad insertion at high scale during live Asia Cup matches, while aligning with Ad Decision Server (ADS) throughput and performance characteristics.”<strong> – </strong>Mukund Acharya, Chief Technology Officer, Sony Pictures Networks India (SPNI).</p>
</blockquote> 
<h3>Architectural Overview</h3> 
<p>The live OTT workflow was architected as a multi-CDN, <a href="https://aws.amazon.com/media-services/" rel="noopener" target="_blank">AWS Elemental media services</a> based SSAI streaming pipeline, designed for scale, personalization, and resiliency:</p> 
<ul> 
 <li>Live Ingest &amp; Encoding with <strong>AWS Elemental MediaLive</strong> 
  <ul> 
   <li>Multiple live inputs were ingested and encoded using <a href="https://aws.amazon.com/medialive/" rel="noopener" target="_blank">MediaLive </a>into adaptive bitrate (ABR) HLS renditions.</li> 
   <li>The encoded outputs were pushed to 3rd party Origin endpoints and <a href="https://aws.amazon.com/mediapackage/" rel="noopener" target="_blank">AWS Elemental MediaPackage</a>, which handled content origination.</li> 
  </ul> </li> 
 <li>Personalized Ad Insertion with <strong>AWS Elemental MediaTailor:</strong> 
  <ul> 
   <li>MediaTailor acted as the SSAI layer, dynamically generating per-session manifests with ad insertions based on user attributes.</li> 
   <li>Integration with Google Ad Manager provided real-time ad decisions via VAST responses.</li> 
   <li>MediaTailor’s PING feature enabled asynchronous ad tracking, ensuring accurate impression reporting without affecting playback or performance.</li> 
   <li>Client SDK on the player side ensured consistent playback and client-side ad beaconing for some specified scenarios.</li> 
  </ul> </li> 
 <li>Content Delivery with CloudFront and multiple 3rd Party CDNs 
  <ul> 
   <li>Final HLS manifests and segments were distributed across multiple CDNs to handle regional traffic spikes and provide low-latency delivery.</li> 
   <li>MediaTailor’s session-based manifest generation ensured that personalization persisted across CDN edges without cache contamination.</li> 
  </ul> </li> 
</ul> 
<p><a href="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2026/03/13/AsiaCup-SSAI.drawio.png" rel="noopener" target="_blank"><img alt="OTT architectural overview" class="size-full wp-image-19720 aligncenter" height="517" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2026/03/13/AsiaCup-SSAI.drawio.png" width="1355" /></a></p> 
<h3>Key AWS Elemental MediaTailor Features That Enabled Scale</h3> 
<ul> 
 <li><strong>Pre-Fetch Optimization:</strong> MediaTailor’s pre-fetch capability was critical in reducing request timeouts and throttling issue at the Ad Decision Server (ADS) during high viewership periods. By resolving ad decisions in advance and keeping those available in MediaTailor before start of each ad-break, SPNI achieved improved fill-rate. The configuration flexibility enabled SPNI to define multiple ad-break types (like, end of over, fall of wicket, drinks break) with distinct pre-fetch strategies for each one, including customizable pre-fetch start points, duration windows, and consumption periods. This distributed ad requests evenly over a defined time duration while aligning with ADS RPS limits and completing ad creative transcoding before the start of ad-break. This approach prevented runtime bottlenecks at ad-break start points by protecting ADS from being overwhelmed by sudden concurrent request spikes. the distributed load model maintained healthy Ad Decision Server performance while delivering improved fill rates and handling millions of personalized ad requests at scale.</li> 
 <li><strong>PING-Based Ad Beaconing for Accurate Impression Tracking:</strong> MediaTailor’s Player Impression Notification Gateway (PING) delivered enterprise-grade ad impression tracking at scale throughout the tournament. Integrated natively within AWS Elemental MediaTailor, the PING framework provided SPNI with the architectural flexibility to deploy a hybrid reporting strategy that intelligently balanced client-side and server-side tracking based on specific operational and measurement needs. Through asynchronous player-generated pings, PING guaranteed dependable ad impression reporting even in challenging network environments while simultaneously minimizing throttling pressure on advertising decision servers. The decoupled architecture ensured tracking calls operated independently from the playback pipeline, eliminating any potential interference with stream continuity and safeguarding the viewer experience. This asynchronous approach also significantly reduced client-side computational overhead compared to conventional synchronous tracking methods. Operating at tournament scale, this architecture successfully and reliably processed billions of tracking beacons to ensure expected ad monetization revenue for SPNI.</li> 
 <li><strong>VisualOn Client SDK Integration:</strong> The integration of the VisualOn Client SDK with AWS Elemental MediaTailor session manifests and tracking endpoints played a key role in ensuring seamless SSAI-based playback across platforms. The SDK enabled robust client-side beaconing and ad tracking while maintaining stream continuity during ad insertions, which is critical in server-side ad insertion scenarios. In addition, the SDK reported detailed client-side playback metrics, allowing the platform to correlate ad impression data with player Quality of Experience (QoE) indicators. This end-to-end visibility strengthened both monetization analytics and playback performance monitoring at scale.</li> 
 <li>Multi-CDN Delivery Architecture Traffic was balanced across multiple CDN providers including CloudFront as one of the prime CDN provider for redundancy and performance optimization. MediaTailor’s stateless, manifest-based personalization model allowed each CDN to serve cached segments while MediaTailor dynamically generated personalized manifests per user session. This approach enabled horizontal scaling of edge delivery without impacting personalization fidelity or increasing origin load.</li> 
 <li>AWS Elemental MediaTailor integrated directly with Google Ad Manager (GAM) over VAST to request and assemble personalized ad pods for each viewer session. This integration combined MediaTailor’s intelligent pre-fetch capabilities with GAM’s ad targeting engine, enabling precise, user-level ad selection based on device type and behavioral segments. During live events, the workflow supported dynamic creative insertion, allowing campaigns to adapt in real time to audience targeting parameters. Additionally, the integration provided real-time visibility into fill-rate performance and campaign pacing, ensuring optimal inventory utilization while maintaining monetization efficiency at scale.</li> 
 <li>By pre-transcoding ad creatives to align with the primary content’s encoding specifications, SPNI achieved seamless transitions between live main content and ads, eliminating playback glitches and minimising buffering incidents to deliver a consistently smooth viewing experience at scale for millions of subscribers.</li> 
</ul> 
<h3>AWS Unified Operations for Media: The Strategic Partner Behind the Scenes</h3> 
<blockquote>
 <p>“AWS Unified Operations for Media along with Solution Architects team worked as a true extension of our team during the Asia Cup. Their designated media streaming experts reviewed our end-to-end workflow and made recommendations to optimize our media pipeline from encoding, ad insertion to delivery. The proactive monitoring, real-time guidance during live matches, and seamless coordination across our partners gave us confidence to deliver flawless experiences to over 10 million concurrent viewers while protecting our advertising revenue. This strategic partnership was instrumental in achieving the scale.” – Mukund Acharya, Chief Technology Office, SPNI, Sony Pictures Networks India</p>
</blockquote> 
<p>The AWS Unified Operations for Media team provided architectural guidance and live operational support to successfully deliver high viewer concurrency cricket streaming event:</p> 
<p><strong>Enhance Streaming Quality&nbsp;with Designated Media Experts:</strong> A designated team of media specialists—including Delivery Manager, Media, Monetization, CloudFront, and Live Event Engineers—worked in tandem with SPNI’s team reviewing control plane, media plane, and data plane architecture, delivering context-aware guidance and real-time support throughout the engagement. The team made recommendations to enhance video quality, improve latency performance and resiliency across all Media Services and CloudFront configurations, ensuring broadcast-grade viewer experiences.</p> 
<p><strong>Optimized Ad Performance:</strong> The Unified Operations for Media team worked very closely with MediaTailor team to validate the performance of the service at scale and collaborated with external partners — Akamai and Google Ad Manager— to optimize workflow for latency, ad fill rate, and ad impression success rate for the high-stake live events.</p> 
<p><strong>Operational Excellence Delivered:</strong> The Unified Operations for Media team developed joint runbooks with SPNI’s operations team and through load testing, proactive monitoring, and live event support, it ensured SPNI achieved operational readiness—delivering flawless streaming experiences throughout the 19 days of the tournament.</p> 
<h3>Benefits/ Outcome:</h3> 
<p>For SPNI, the result was a smooth, monetizable live-streaming distribution at scale. The workflow maintained broadcast-grade latency with seamless mid-roll transitions and stable ad stitching under high concurrency conditions. This deployment redefined what’s possible in live OTT advertising, setting a new benchmark for SPNI’s streaming capabilities.</p> 
<ul> 
 <li><strong>Increased Ad Revenue:</strong> Through real-time personalization and better fill rates.</li> 
 <li><strong>Reduce Ad Revenue Risk:</strong> MediaTailor PING feature allows beacon retries for up to an hour to protect ad revenue allowing recovery time for a downstream ADS</li> 
 <li><strong>Improved Viewer Experience:</strong> Smooth ad transitions, zero buffering.</li> 
 <li><strong>Scalable Global Delivery:</strong> Multi-CDN architecture with MediaTailor handled regional peaks effortlessly.</li> 
 <li><strong>Operational Simplicity:</strong> A managed, serverless architecture reduced complexity and cost.</li> 
</ul> 
<h3>Lessons Learned</h3> 
<p>Based on operational experience of live SSAI workloads for high-concurrency Asia cup event, the following insights were observed:</p> 
<ul> 
 <li>Pre-Fetching ad decisions significantly reduces latency at scale; without it, live SSAI can easily bottleneck under ad server’s RPS limitations.</li> 
 <li>PING beacons are more reliable than synchronous client tracking for maintaining accurate metrics during high concurrency live sports events which causes network congestion at ADS.</li> 
 <li>A multi-CDN setup provides both redundancy and geo-optimization benefits without compromising personalization.</li> 
 <li>Close coordination between the player SDK, MediaTailor configuration, and ad server logic is essential for large-scale SSAI success.</li> 
</ul> 
<blockquote>
 <p>“AWS is proud to partner with SPNI in transforming how live sports streaming drives business value. Elemental MediaServices which includes MediaTailor in SPNI’s Asia Cup deployment achieved exceptional monetization at scale—maximizing ad fill rates while maintaining premium viewing experiences. AWS Elemental MediaTailor – fully managed, serverless approach along with pre-fetch feature, fundamentally changes the economics of large-scale live sports advertising that aligns with the unpredictable nature of live events”. – Abhijeet Pandey, Head of Core Services, AWS India</p>
</blockquote> 
<h3>Conclusion</h3> 
<p>This deployment demonstrates how <a href="https://aws.amazon.com/mediatailor/" rel="noopener" target="_blank">AWS Elemental MediaTailor,</a> combined with <a href="https://aws.amazon.com/medialive/" rel="noopener" target="_blank">AWS Elemental MediaLive</a>, 3rd Party Origin server, <a href="https://aws.amazon.com/mediapackage/" rel="noopener" target="_blank">AWS Elemental MediaPackage</a>, and multi-CDN delivery, can support live streaming of large live sporting events with real-time ad personalization. By leveraging Pre-Fetch, PING tracking, and client-side SDK integration, the customer achieved broadcast-quality ad delivery at unprecedented scale for concurrent users with precision and reliability.</p> 
<h3>Ready to Scale Your Streaming Workload on AWS?</h3> 
<p><a href="https://aws.amazon.com/contact-us/" rel="noopener" target="_blank">Contact AWS today</a> to learn how MediaTailor and Unified Operations for Media can help you deliver personalized ad experiences that engage viewers and drive revenue growth.</p> 
<p><strong>Learn more</strong> about <a href="https://aws.amazon.com/mediatailor" rel="noopener" target="_blank">AWS Elemental MediaTailor</a> and how it enables personalized streaming experiences at scale.</p> 
<p><strong>Learn more</strong> about <a href="https://aws.amazon.com/premiumsupport/plans/unified-operations/media/" rel="noopener" target="_blank">Unified Operations for Media</a> and how it supports seamless content delivery at any scale.</p>
