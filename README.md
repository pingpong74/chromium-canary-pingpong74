# Chromium Canary Submission — Arpan Bajpai (@pingpong74)

## 1. Loom Walkthrough Video
* **Video URL:** [Watch 5-Minute Walkthrough](https://www.youtube.com/watch?v=FURji8dFObY)
* Note: the video temporarily uses `[CANARY_ACK_LIVE]` to show a live rebuild; the submitted patch uses the required `[CANARY_ACK]`.

## 2. Build Info
* **Chromium Commit ID (`git rev-parse HEAD`):** `95e805b749ecfec416870ada019531d63ef90190`
* **Target Built:** `chrome`
* **My Laptop OS & RAM:** `Arch linux (Linux LTS 6.18.59), 16 GB RAM (used 19GB Swap)`

## 3. Architecture Notes
* **Feature A (`X-Candidate-Canary` Header):** `"inject-candidate-id" is added to the kSwitchNames allowlist in PropagateBrowserCommandLineToRenderer, so the switch is copied from the Browser process's command line to renderer processes. A class CanaryThrottle : public blink::URLLoaderThrottle sets X-Candidate-Canary in cors_exempt_headers for http/https requests. It exists in two files because it runs in two processes: in ChromeContentBrowserClient::CreateURLLoaderThrottles (Browser process) and in URLLoaderThrottleProviderImpl::CreateThrottles (Blink Renderer). Each only adds the throttle when the switch is non-empty. Finally, the header name (X-Candidate-Canary) is registered in the cors_exempt_header_list in StoragePartitionImpl::InitNetworkContext, because the network service rejects exempt headers that aren't on that list.`
* **Feature B (`[CANARY_ACK]` Console Hook):** `the frame/console.cc did not exist but a similar file which does the same job called frame/frame_console.cc was found through which console.warn goes. Over there, inside the function FrameConsole::ReportMessageToClient, I simply checked for the required warning level and message and logged the required output as a warning. `

## 4. AI Agent Workflow
* **AI Tool & Model Used:** `Opencode with Deepseek 4.1`
* **How I Used Sub-Agents:** `I created two read-only sub-agents, blink-search and ipc-flags-search, with specific instructions on what to search and which tools to use. They traced the console.warn path in Blink and the command-line propagation and URLLoaderThrottle creation paths. I reviewed their reports, wrote the edit instructions, and the main agent made the changes. My free quota then ran out, so I fixed the CORS issue by manually registering X-Candidate-Canary in the cors_exempt_header_list. Logs and agents are in ai_transcripts/.`
