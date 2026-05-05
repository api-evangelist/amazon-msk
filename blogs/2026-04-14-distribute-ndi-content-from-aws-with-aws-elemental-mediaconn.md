---
title: "Distribute NDI content from AWS with AWS Elemental MediaConnect"
url: "https://aws.amazon.com/blogs/media/distribute-ndi-content-from-aws-with-aws-elemental-mediaconnect/"
date: "Tue, 14 Apr 2026 14:41:11 +0000"
author: "Michael Henry"
feed_url: "https://aws.amazon.com/blogs/media/feed/"
---
<p>The secure and reliable egress of live NDI® (<a href="https://ndi.video/tech/" rel="noopener" target="_blank">Network Device Interface</a>) video content from the <a href="https://aws.amazon.com/" rel="noopener" target="_blank">Amazon Web Services (AWS)</a> Cloud has previously been a challenge for our customers. With the addition of NDI source support in <a href="https://docs.aws.amazon.com/mediaconnect" rel="noopener" target="_blank">AWS Elemental MediaConnect</a>, you can now take live NDI video within an <a href="https://aws.amazon.com/vpc" rel="noopener" target="_blank">AWS Virtual Private Cloud (Amazon VPC)</a> and seamlessly distribute it as a transport stream.</p> 
<p>NDI is a high-quality video connectivity technology that facilitates ultra-low latency video communication over internet protocol (IP). NDI is widely used in live production applications and supported by more than 500 hardware products and 400 software applications. Systems use NDI to encode, transmit, and receive multiple streams of video and audio in near real-time.</p> 
<p>The new <a href="https://aws.amazon.com/mediaconnect/" rel="noopener" target="_blank">AWS Elemental MediaConnect</a> functionality that converts NDI sources to Secure Reliable Transport (SRT) or other compressed stream outputs provides an efficient way to bridge NDI with modern, transport stream-based distribution tools. Broadcasters and live production teams can use this capability to seamlessly integrate the flexibility of NDI-based production environments into their existing, standards-based distribution workflows.</p> 
<p>In this post, we show you how to build a demonstration of low-latency live NDI video conversion to a transport stream in the AWS Cloud with MediaConnect. You will:</p> 
<ul> 
 <li>Create an <a href="https://aws.amazon.com/vpc" rel="noopener" target="_blank">Amazon Virtual Private Cloud (Amazon VPC)</a>.</li> 
 <li>Create a MediaConnect <a href="https://docs.aws.amazon.com/mediaconnect/latest/ug/flows.html" rel="noopener" target="_blank">flow</a> with an NDI source in the Amazon VPC.</li> 
 <li>Create an <a href="https://aws.amazon.com/medialive" rel="noopener" target="_blank">Amazon Elemental MediaLive</a> channel to receive MediaConnect output.</li> 
 <li>Create a MediaConnect output to MediaLive.</li> 
 <li>Create an <a href="https://aws.amazon.com/mediapackage" rel="noopener" target="_blank">Amazon Elemental MediaPackage</a> output group to receive from MediaLive.</li> 
 <li>Create a Windows <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html" rel="noopener" target="_blank">Amazon Elastic Compute Cloud (Amazon EC2)</a> instance in the Amazon VPC, install NDI Tools, an NDI Discovery Server and <a href="https://kanelp7.wixsite.com/kanesworld/ndi-signal-generator" rel="noopener" target="_blank">NDI Signal Generator</a>.</li> 
 <li>Use the NDI Signal Generator to stream video and audio data over NDI in the Amazon VPC to MediaConnect.</li> 
</ul> 
<p>MediaConnect is a transport service that enables secure and reliable delivery of live video into, within, and out of the AWS Cloud. Using MediaConnect, you can send high-quality video to AWS for processing, storage, and distribution, with features including encryption, redundancy, and bandwidth-efficient protocols for reliable video transport.</p> 
<p>The following diagram (Figure 1) shows how you can use the NDI Signal Generator to stream content over NDI in an Amazon VPC in the AWS Cloud. A Windows EC2 instance in the Amazon VPC, running NDI Tools Discovery Server, delivers the NDI content to MediaConnect. MediaConnect converts the NDI content to a transport stream output to MediaLive. MediaLive transcodes and sends the stream to MediaPackage. You can then use the MediaPackage console to view the transcoded media through the MediaPackage origin endpoint.</p> 
<p style="text-align: center;"><a href="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2026/04/02/Figure-1-High-level-architecture_NDI.png" rel="noopener" target="_blank"><img alt="Diagram showing a simplified NDI signal distribution in the AWS Cloud. An EC2 instance in the Amazon VPC delivers NDI content from an NDI Signal Generator to MediaConnect. MediaConnect converts the NDI content to a transport stream and distributes the content through MediaLive and MediaPackage." class="size-full wp-image-19762 aligncenter" height="461" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2026/04/02/Figure-1-High-level-architecture_NDI.png" width="681" /></a></p> 
<h3 style="text-align: center;"><em>Figure 1: High-level architecture</em></h3> 
<h3>The workflow</h3> 
<p>Within your private Amazon VPC, a public subnet hosts a Windows EC2 instance running the NDI Discovery Server and NDI Signal Generator. The NDI Signal Generator application acts as the NDI sender, outputting an NDI stream to a MediaConnect flow. A dedicated VPC interface handles MediaConnect NDI ingress traffic privately within the VPC. MediaConnect then converts the NDI feed to a transport stream for distribution through MediaLive and MediaPackage.</p> 
<p>The AWS resources to enable this workflow are set up using <a href="https://aws.amazon.com/cloudformation/" rel="noopener" target="_blank">AWS CloudFormation</a> templates. CloudFormation is an infrastructure as code (IaC) service. Templates let you model, provision, and manage AWS and third-party resources as code. For more information about using CloudFormation templates, see <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html" rel="noopener" target="_blank">Working with CloudFormation templates</a>.</p> 
<h3>Prerequisites</h3> 
<p>To set up the solution in this post, you must have the following:</p> 
<ul> 
 <li>An AWS account that has permission to create a GPU instance, which isn’t enabled by default.</li> 
 <li>A service quota of at least four vCPUs for running on-demand G and VT instances to support a four vCPU g4dn.xlarge EC2 instance. To check your quota, use the <a href="https://us-west-2.console.aws.amazon.com/servicequotas/home/services/ec2/quotas" rel="noopener" target="_blank">EC2 Service Quota console</a>.</li> 
</ul> 
<p>This workflow is for demonstration purposes. For production workflows, apply additional security measures (such as permissions and CIDR blocks) for your specific environment.</p> 
<h3>The solution</h3> 
<p>With the prerequisites in place, you’re ready to set up and test the solution.</p> 
<h3>Step 1. Create the AWS Cloud resources</h3> 
<p>Deploy the AWS resources from a system that contains the <a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html" rel="noopener" target="_blank">AWS Command Line (AWS CLI) tools</a>. Confirm the <a href="https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-files.html" rel="noopener" target="_blank">AWS account credentials and default AWS Regio</a>n are set up.</p> 
<p>1. Download the <a href="https://github.com/aws-samples/sample-MediaConnect-Distribute-Content-Ndi" rel="noopener" target="_blank">CloudFormation templates and scripts</a>.</p> 
<p>2. Create an Amazon Simple Storage Service (Amazon S3) bucket in your AWS account to store the CloudFormation template files.</p> 
<p>3. To create the AWS Cloud resources, create a CloudFormation stack using the following command (be certain to replace the values in this bulleted list accordingly):</p> 
<ul> 
 <li><strong>&lt;S3_BUCKET_NAME&gt;:</strong> The name of your S3 bucket.</li> 
 <li><strong>&lt;S3_FOLDER&gt;:</strong> The name of the folder to create within the S3 bucket.</li> 
 <li>&lt;<strong>STACK_NAME&gt;:</strong> The name of the stack to create.</li> 
 <li>&lt;<strong>PREFIX&gt;:</strong> The prefix for new AWS resource names.</li> 
 <li><strong>&lt;RDP_CIDR_BLOCK&gt;:</strong> The CIDR block for the IP range for remote desktop access to the EC2 instance.</li> 
 <li><strong>(Optional) &lt;IAM_PROFILE&gt;:</strong> The <a href="https://aws.amazon.com/iam" rel="noopener" target="_blank">AWS Identity and Access Management (IAM)</a> profile to use instead of default AWS credentials. Default credentials require a region. Note, the <code>.aws/credentials</code> file must contain the profile and default Region.</li> 
</ul> 
<p><strong>Linux command:</strong></p> 
<div class="hide-language"> 
 <pre><code class="lang-json">./create_stack.sh &lt;S3_BUCKET_NAME&gt; &lt;S3_FOLDER&gt; &lt;STACK_NAME&gt; &lt;PREFIX&gt; \
&lt;RDP_CIDR_BLOCK&gt; &lt;IAM_PROFILE&gt;
</code></pre> 
</div> 
<p><strong>Windows PowerShell command:</strong></p> 
<div class="hide-language"> 
 <pre><code class="lang-json">./create_stack.ps1 &lt;S3_BUCKET_NAME&gt; &lt;S3_FOLDER&gt; &lt;STACK_NAME&gt; &lt;PREFIX&gt; \
&lt;RDP_CIDR_BLOCK&gt; &lt;IAM_PROFILE&gt;
</code></pre> 
</div> 
<p>4. If the process completes successfully, the script outputs the following information:</p> 
<ul> 
 <li>FlowArn is the <a href="https://docs.aws.amazon.com/mediaconnect/latest/ug/what-is-concepts.html" rel="noopener" target="_blank">Amazon Resource Name (ARN)</a> of the MediaConnect flow.</li> 
 <li>The name of the EC2 instance keypair’s <code>PEM</code> file written to your current folder. The file is used in the next section.</li> 
</ul> 
<h3>Step 2. Install NVIDIA driver on the Windows EC2 instance</h3> 
<p>The NDI Studio Monitor application requires a GPU instance and a specific set of NVIDIA drivers. The CloudFormation template creates the GPU instance and downloads the NVIDIA driver installer application to the desktop. The application requires manual intervention to install the driver. Use the following to install the driver:</p> 
<p>1. <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-rdp.html#retrieve-initial-admin-password" rel="noopener" target="_blank">Retrieve the Administrator password</a> for the instance using the <code>PEM</code> file from Step 1.</p> 
<p>2. <a href="https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html" rel="noopener" target="_blank">Sign in</a> to the instance.</p> 
<p>3. Navigate to the desktop and open the <strong>NVIDIA</strong> folder.</p> 
<p>4. Continue to open the subfolders until you locate the installation executable file. The following is an example of the name:<br /> <code>XXX.XX_Cloud_Gaming_win10_win11_server2022_dch_64bit_international.exe</code></p> 
<p>5. Open the file to launch it.</p> 
<p>6. Follow the prompts to install.</p> 
<h3>Step 3. Install NDI Tools and launch the NDI Discovery Server</h3> 
<p>NDI distributes a set of tools that are free to download and use.</p> 
<p>1. Download and install <a href="https://ndi.video/tools/" rel="noopener" target="_blank">NDI Tools</a>.</p> 
<p>2. Launch the Discovery Server application from:</p> 
<p><code>C:\Program Files\NDI\NDI 6 Tools\Discovery Service\NDI Discovery Service.exe</code></p> 
<p>3. The server runs using a shell to display the output.</p> 
<p>4. (Optional) To automatically launch the Discovery Server on system restarts, create a program shortcut and move it to:</p> 
<p><code>C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup</code></p> 
<h3>Step 4. Configure NDI Discovery Server</h3> 
<p>Use NDI Tool’s Access Manager to configure the Discovery Server IP address, as shown in Figure 2, replacing <code>XXX.XXX.XXX.XXX</code> with the <strong>Private IP</strong> address of the Windows EC2 instance. Because the Discovery Server is on the instance, you can optionally specify the <strong>localhost</strong> instead of the IP address.</p> 
<p><a href="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2026/04/02/Figure-2-Set-Discovery-Server-IP-address-using-NDI-Access-Manager.jpg" rel="noopener" target="_blank"><img alt="Configure Discovery Server IP address using NDI Tool’s Access Manager user interface. The Advanced tab is selected. Under Network Mapping, Discovery Servers is selected and the IP Addresses XXX.XXX.XXX.XXX is highlighted in a red box." class="size-full wp-image-19761 aligncenter" height="417" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2026/04/02/Figure-2-Set-Discovery-Server-IP-address-using-NDI-Access-Manager.jpg" width="458" /></a></p> 
<p style="text-align: center;"><em>Figure 2: Set Discovery Server IP address using NDI Access Manager</em></p> 
<h3>Step 5. Install NDI Signal Generator on the Windows EC2 instance and create NDI source</h3> 
<p>1. Download and install an <a href="https://kanelp7.wixsite.com/kanesworld/ndi-signal-generator" rel="noopener" target="_blank">NDI Signal Generator</a> application on the Windows instance.</p> 
<p>2. Add an inbound rule to the Windows firewall to allow the NDI Signal Generator application to receive network traffic. You can add the rule using the Windows Defender Firewall with Advanced Security application, or by running the following command using an administrator command prompt:</p> 
<div class="hide-language"> 
 <pre><code class="lang-json">netsh advfirewall firewall add rule name="NDI_SignalGenerator" dir=in action=allow program="C:\Program Files (x86)\NDI Signal Generator\NDI Signal Generator.exe" enable=yes</code></pre> 
</div> 
<p>3. Launch the application.</p> 
<p>4. Start the NDI source. Note the <strong>NDI Channel Name</strong>. It will be used in the next step as the <strong>NDI source name</strong>.</p> 
<h3>Step 6. Set the NDI source name and start the MediaConnect flow (console)</h3> 
<p>1. Open the <a href="https://console.aws.amazon.com/medialive/" rel="noopener" target="_blank">AWS Elemental MediaConnect console</a>.</p> 
<p>2. Navigate to Flows and select the <strong>&lt;PREFIX&gt;Flow</strong> flow.</p> 
<p>3. Under the <strong>Sources</strong> tab, select the NDI source.</p> 
<p>4. Choose <strong>Update</strong> and enter the name of the NDI source from the previous step. It should be something like <code>EC2AMAZ-XXXXXXX (SIGNAL)</code>.</p> 
<p>5. Choose <strong>Update source</strong>.</p> 
<p>6. Choose <strong>Start</strong> to start the flow.</p> 
<p>7. After the flow starts, the stream from the NDI Signal Generator application is visible in the flow’s thumbnail preview window.</p> 
<h3>Step 7. Start the MediaLive channel (console)</h3> 
<p>1. Open the <a href="https://console.aws.amazon.com/medialive/" rel="noopener" target="_blank">AWS Elemental MediaLive console</a>.</p> 
<p>2. Navigate to <strong>Channels</strong> and select the <strong>&lt;PREFIX&gt;LiveChannel</strong> channel.</p> 
<p>3. Choose <strong>Start</strong>.</p> 
<p>4. After the channel starts, the stream from the MediaConnect is visible in the channel’s thumbnail preview window.</p> 
<h3>Step 8. Use MediaPackage to preview the stream</h3> 
<p>1. Open the <a href="https://console.aws.amazon.com/mediapackage/" rel="noopener" target="_blank">AWS Elemental MediaPackage console</a>.</p> 
<p>2. Navigate to <strong>Live V2, Channel groups</strong> and select the &lt;<strong>PREFIX&gt;Group</strong> channel group.</p> 
<p>3. On the <strong>Channels</strong> tab, select the <strong>&lt;PREFIX&gt;Channel</strong> channel.</p> 
<p>4. Under <strong>Origin endpoints</strong>, choose the radio button for the endpoint.</p> 
<p>5. Under <strong>Manifests</strong>, choose the radio button for the manifest.</p> 
<p>6. Under <strong>Manifests</strong>, select <strong>Preview</strong>. The stream from MediaPackage is visible in the console window.</p> 
<h3>Clean up</h3> 
<p>To prevent charges after evaluating this solution, stop the flow and delete all the AWS resources created for the solution by using the following command (replace the values in this bulleted list accordingly):</p> 
<ul> 
 <li><strong>&lt;STACK_NAME&gt;:</strong> The name of the stack to delete.</li> 
 <li>(Optional) <strong>&lt;IAM_PROFILE&gt;:</strong> The IAM Profile to use instead of default AWS credentials. Default credentials require a region. Note, the <code>.aws/credentials</code> file must contain the profile and default region.</li> 
</ul> 
<p><strong>Linux command:</strong></p> 
<p><code>./delete_stack.sh &lt;STACK_NAME&gt; &lt;IAM_PROFILE&gt;</code></p> 
<p>Windows PowerShell command:</p> 
<p><code>.\delete_stack.ps1 &lt;STACK_NAME&gt; &lt;IAM_PROFILE&gt;</code></p> 
<p>Verify that the AWS resources created for the solution no longer exist by checking the AWS console. This confirms no resources are accidentally left running, which could lead to additional charges.</p> 
<h3>Conclusion</h3> 
<p>In this post, you learned how to use AWS Elemental MediaConnect to convert an NDI source within an Amazon VPC into a transport stream and distribute it through MediaLive and MediaPackage. This facilitates communication between NDI enabled software and modern IP based live cloud production workflows.</p> 
<p>Contact an <a href="https://pages.awscloud.com/Media-and-Entertainment-Contact-Us.html" rel="noopener" target="_blank">AWS representative</a> to learn how AWS can help accelerate your business.</p> 
<h3>Further reading</h3> 
<ul> 
 <li><a href="https://aws.amazon.com/about-aws/whats-new/2026/03/mediaconnect-ndi-inputs/" rel="noopener" target="_blank">AWS Elemental MediaConnect adds support for NDI inputs</a></li> 
 <li><a href="https://aws.amazon.com/solutions/guidance/programmatic-deployment-of-ndi-discovery-servers-for-broadcast-workflows-on-aws/" rel="noopener" target="_blank">Guidance for Programmatic Deployment of NDI Discovery Servers for Broadcast Workflows on AWS</a></li> 
 <li><a href="https://aws.amazon.com/blogs/media/using-ndi-for-video-switching-and-routing-in-aws-2/" rel="noopener" target="_blank">Using NDI for video switching and routing in AWS</a></li> 
 <li><a href="https://aws.amazon.com/blogs/media/aws-supports-the-second-multi-partner-live-cloud-production-interoperability-workshop/" rel="noopener" target="_blank">AWS supports the second multi-partner live cloud production interoperability workshop</a></li> 
 <li><a href="https://docs.ndi.video/all" rel="noopener" target="_blank">NDI Docs &amp; Guides</a></li> 
</ul>
