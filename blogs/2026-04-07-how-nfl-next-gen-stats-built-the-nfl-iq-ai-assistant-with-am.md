---
title: "How NFL Next Gen Stats Built the NFL IQ AI Assistant with Amazon Quick"
url: "https://aws.amazon.com/blogs/media/how-nfl-next-gen-stats-built-the-nfl-iq-ai-assistant-with-amazon-quick/"
date: "Tue, 07 Apr 2026 12:23:23 +0000"
author: "Ari Entin"
feed_url: "https://aws.amazon.com/blogs/media/feed/"
---
<p><em>NFL IQ’s conversational approach to football data replaces tool navigation with natural conversation across stats, models, and team insights so fans get instant answers.</em></p> 
<p>Every NFL offseason runs on data. Player tracking from NFL Next Gen Stats, draft models, free agency movement, roster construction, and historical comparisons all feed into how teams evaluate decisions. Over time, that ecosystem has grown more sophisticated, layering predictive models and contextual metrics on top of raw data.</p> 
<p>For fans, however, that evolution has introduced a different kind of complexity.</p> 
<p>The challenge has not been a lack of insight—it has been how that insight is accessed. Even straightforward questions require navigating multiple surfaces, each designed around a specific function: mock drafts, depth charts, team needs, historical trends. The burden has consistently been on fans to move between them, interpret outputs, and assemble a coherent view.</p> 
<p>“The amount of information can be overwhelming at times, and it’s often published in many different places,” said Mike Band of NFL Next Gen Stats Research &amp; Analytics. “One of the key goals was to bring it all together into one powerful dashboard.”</p> 
<p><a href="https://www.nfl.com/iq">NFL IQ</a>, a living offseason intelligence platform, addresses that problem not by reducing the complexity of the underlying system, but by changing how people interact with it.</p> 
<h2>Embedding AI directly into the analytics layer</h2> 
<p>At the center of NFL IQ is a native AI assistant embedded directly within the experience. It is built alongside the same data and analytics infrastructure rather than layered on top of it.</p> 
<p>The system is powered by <a href="https://aws.amazon.com/quick/">Amazon Quick</a>, which acts as both the analytics layer and the foundation for integrating AI-driven interaction into the product. Dashboards, structured data, and conversational capabilities operate within the same environment. This allows the assistant to work from the same source of truth as the visual analytics layer.</p> 
<p>“We didn’t want to build a chatbot that sits off to the side,” said Band. “The goal was to embed the assistant into the same layer that powers the dashboards, so it operates on the same data models.”</p> 
<p>This architectural decision changes how the assistant behaves. It is not querying a loosely connected set of APIs or external sources; it is operating within the same system that powers the dashboards themselves. That proximity allows it to access structured datasets, contextual metadata, and curated content in a coordinated way. This translates into faster responses, more accurate answers, and a unified experience where the assistant becomes a native interface to the platform’s intelligence rather than a helpful add-on.</p> 
<p>“What’s powerful is that it’s not only accessing the data in the dashboard, but also connecting to dozens of broader information sources,” said Band.</p> 
<p>The result is a system that can move fluidly between different types of information—structured data, editorial context, and explanatory definitions—while maintaining consistency in how answers are generated.</p> 
<h2>Building a retrieval system that can reason across sources</h2> 
<p>One of the core technical challenges in building the NFL IQ assistant was not retrieval—it was orchestration.</p> 
<p>The system needs to determine which data sources are relevant to a given question, how those sources relate to one another, and how to synthesize them into a response that reflects both the data and the context around it.</p> 
<p>To achieve that, the team avoided a “connect everything at once” approach and instead built the system incrementally through Quick.<br /> “Rather than starting with everything, we added data sources incrementally so we could evaluate how well the model uses the data and ensures it consistently adds value,” said Keegan Abdoo, Senior Manager at Next Gen Stats.</p> 
<p>This approach allowed them to test how effectively the assistant could route queries across different inputs—structured datasets powering the dashboard, curated content from nfl.com, and the definitions that explain how metrics are constructed.<br /> Context emerged as a critical component of that system.</p> 
<p>“The definitions and explanations around the data are just as important,” Abdoo said. “That context helps the system understand which data to use, why it matters, and what should be surfaced.”</p> 
<p>By incorporating both data and its meaning, the assistant can produce responses that reflect reasoning rather than retrieval.</p> 
<p>“By combining multiple sources, we’re able to consistently provide insightful answers that help fans become smarter and anticipate second and third-order analysis,” said Band. “This effectively turns the system into a reasoning engine, where answers are constructed through the interaction of multiple inputs rather than pulled from a single source.”</p> 
<h2>Modeling the draft as a dynamic system</h2> 
<p>The draft provides a clear example of how that architecture operates in practice.</p> 
<p>At a surface level, predicting a team’s pick might appear to be a ranking problem. In reality, it’s a dynamic system shaped by multiple variables: available players, roster composition, positional value, historical decision patterns, and the sequence of picks leading up to a selection.</p> 
<p>“The simplest question is just, ‘Who will my team draft?’” said Abdoo.</p> 
<p>Answering that question requires the system to evaluate probabilities in context, not in isolation. Fans can ask, ‘If a player isn’t available at a certain pick, who becomes the next best fit?’ and it allows you to game out the draft as it changes.</p> 
<p>Each query effectively modifies the state of the system, requiring the assistant to recompute relevance based on updated constraints. That includes incorporating patterns that are not explicitly encoded as single metrics.</p> 
<p>“You can ask about GM tendencies, draft history, and the types of players a team has targeted—and how that applies to this year’s class,” said Band. “The assistant is not simply retrieving a prediction—it is constructing a response that reflects how those variables interact.”<br /> Translating complexity into accessible reasoning</p> 
<p>A key part of the system’s design is its ability to translate complex outputs into explanations that are accessible without oversimplifying the underlying logic.</p> 
<p>“You don’t have to be an expert or even know where to find the information,” said Band. “You can just ask a question and get your answer back.”</p> 
<p>Under the hood, Quick handles the complexity of routing queries, evaluating relevant inputs, and generating a response. The output, however, is structured in a way that aligns with how users think about the problem.</p> 
<p>“If it’s a simple question, you’ll get an answer quickly. If it requires deeper reasoning, it will take more time—but the goal is to handle both and deliver answers in a way fans can understand,” said Abdoo.</p> 
<p>That balance—between technical depth and accessibility—is central to the experience. It allows the system to support a wide range of use cases, from quick queries to more complex scenario analysis, without requiring the user to adjust how they interact with it.</p> 
<h2>Replacing navigation with a reasoning interface</h2> 
<p>NFL IQ represents a shift in how complex analytics systems are experienced.</p> 
<p>Instead of requiring users to navigate tools and interpret outputs, the system operates as an interface for reasoning—taking a question as input and applying data, models, and context to produce an answer.</p> 
<p>By embedding that capability directly within the analytics layer through Quick, the NFL has effectively collapsed multiple workflows into a single interaction model.</p> 
<p>The result is not a simplification of the data, but a reconfiguration of how it is accessed—moving from navigation to interaction, and from isolated outputs to connected explanations.</p> 
<p>In that sense, NFL IQ is less about adding a new feature and more about redefining the interface to the NFL offseason itself.</p>
