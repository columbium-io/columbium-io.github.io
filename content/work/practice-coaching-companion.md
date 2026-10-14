---
title: "Coaching Feedback from a Phone Video"
date: 2026-10-13
summary: "A web app that changes a 15-minute practice video into coaching feedback for each goal. It uses multimodal AI and keeps video private."
ShowReadingTime: true
---

<p class="lede">The AI model call is the easy part of an AI product. Cost, privacy and testing are the hard parts.</p>

<div class="stats">
<div><b>15 min</b><span>practice video in, feedback for each goal out</span></div>
<div><b>4</b><span>end-to-end browser test suites</span></div>
<div><b>0</b><span>external calls in the browser tests</span></div>
<div><b>1</b><span>command to deploy</span></div>
</div>

## The product

1. A parent makes an improvement plan for a young athlete. The plan has phases and goals.
2. The parent records a 15-minute practice video on a phone. The recording stops automatically.
3. A multimodal AI model compares the video to the plan.
4. The parent gets a score and specific feedback for each goal.

## Our engineering decisions

- **Stream the video.** The server streams the upload directly to a temporary file. It never keeps the full video in memory. Thus, small and cheap cloud containers are sufficient.
- **Better quality at the same AI cost.** The AI provider charges for video length, not file size. Thus, we increased the video quality at no added AI cost. We also removed a second, unnecessary compression step.
- **Keep video private.** The system is designed to delete all copies of the video after the analysis. This includes the copy at the AI provider. The deletion also occurs when the analysis fails.
- **Test without the cloud.** One command starts the full system on a local computer. A test version of the AI model gives the same answer each time. Thus, the browser tests run with no external calls and no cost.
- **Deploy with one command.** A script sets the permissions, deploys the services and checks the result.

## What this means for you

An AI feature must have good economics, safe data and proof that it works. We engineer these properties together with the model integration.

<div class="cta">
<strong>Do you want to add AI to your product?</strong>
<p>Tell us about your users. We reply with a plan, a cost estimate per use and a test strategy.</p>
<a class="btn" href="mailto:hello@columbium.io">Start a project →</a>
</div>
