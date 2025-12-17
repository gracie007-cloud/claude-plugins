---
name: chrome-dev-tools
description: "Interact with Chrome browser for testing and debugging. Use when asked to test in browser, debug web pages, take screenshots, inspect network requests, analyze performance, or automate browser interactions."
allowed-tools: mcp__chrome-devtools__click, mcp__chrome-devtools__drag, mcp__chrome-devtools__fill, mcp__chrome-devtools__fill_form, mcp__chrome-devtools__handle_dialog, mcp__chrome-devtools__hover, mcp__chrome-devtools__upload_file, mcp__chrome-devtools__navigate_page, mcp__chrome-devtools__new_page, mcp__chrome-devtools__list_pages, mcp__chrome-devtools__select_page, mcp__chrome-devtools__close_page, mcp__chrome-devtools__navigate_page_history, mcp__chrome-devtools__wait_for, mcp__chrome-devtools__evaluate_script, mcp__chrome-devtools__list_console_messages, mcp__chrome-devtools__take_screenshot, mcp__chrome-devtools__take_snapshot, mcp__chrome-devtools__list_network_requests, mcp__chrome-devtools__get_network_request, mcp__chrome-devtools__performance_start_trace, mcp__chrome-devtools__performance_stop_trace, mcp__chrome-devtools__performance_analyze_insight, mcp__chrome-devtools__emulate_cpu, mcp__chrome-devtools__emulate_network, mcp__chrome-devtools__resize_page
---

# Chrome DevTools Skill

Interact with Chrome browser for testing, debugging, and automation using Chrome DevTools Protocol via Puppeteer.

## Prerequisites

- Node.js 22+
- Chrome browser installed (stable version)

## Connection Options

By default, the MCP server starts a new Chrome instance with a dedicated profile.

### Connect to Existing Chrome

To share state between manual and agent-driven testing:

1. **Automatic connection** (Chrome 144+): Shares state automatically
2. **Manual connection**: Start Chrome with remote debugging:
   ```bash
   /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222
   ```

## Available Tools

### Input Automation (7 tools)
| Tool | Purpose |
|------|---------|
| `click` | Click on element |
| `drag` | Drag element from one location to another |
| `fill` | Fill input field with text |
| `fill_form` | Fill multiple form fields at once |
| `handle_dialog` | Accept or dismiss browser dialogs |
| `hover` | Hover over element |
| `upload_file` | Upload file to file input |

### Navigation (7 tools)
| Tool | Purpose |
|------|---------|
| `navigate_page` | Navigate to URL |
| `new_page` | Open new browser tab |
| `list_pages` | List all open pages/tabs |
| `select_page` | Switch to specific tab |
| `close_page` | Close current tab |
| `navigate_page_history` | Go back/forward in history |
| `wait_for` | Wait for element or condition |

### Debugging (4 tools)
| Tool | Purpose |
|------|---------|
| `evaluate_script` | Execute JavaScript in page context |
| `list_console_messages` | Get console output |
| `take_screenshot` | Capture page screenshot |
| `take_snapshot` | Take DOM snapshot |

### Network (2 tools)
| Tool | Purpose |
|------|---------|
| `list_network_requests` | List all network requests |
| `get_network_request` | Get details of specific request |

### Performance (3 tools)
| Tool | Purpose |
|------|---------|
| `performance_start_trace` | Start performance recording |
| `performance_stop_trace` | Stop recording and get trace |
| `performance_analyze_insight` | Analyze trace for insights |

### Emulation (3 tools)
| Tool | Purpose |
|------|---------|
| `emulate_cpu` | Throttle CPU speed |
| `emulate_network` | Simulate network conditions |
| `resize_page` | Change viewport size |

## Common Workflows

### Debug Console Errors
1. Navigate to page with `navigate_page`
2. Check console with `list_console_messages`
3. Take screenshot with `take_screenshot` to capture state

### Inspect Network Issues
1. Navigate to page
2. List requests with `list_network_requests`
3. Get details of failing request with `get_network_request`

### Performance Analysis
1. Start trace with `performance_start_trace`
2. Perform actions on page
3. Stop trace with `performance_stop_trace`
4. Analyze with `performance_analyze_insight`

### Form Testing
1. Navigate to form page
2. Fill form with `fill_form`
3. Click submit with `click`
4. Verify result with `take_screenshot` or `evaluate_script`

### Mobile Testing
1. Resize viewport with `resize_page` to mobile dimensions
2. Optionally throttle with `emulate_cpu` and `emulate_network`
3. Navigate and test
