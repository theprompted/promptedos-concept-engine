# HTML Output Template

Use this template for the concepts output file.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[Product] Ad Concepts — [Date]</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: #1a1a2e;
            color: #e0e0e0;
            line-height: 1.6;
            padding: 20px;
        }
        .header {
            max-width: 800px;
            margin: 0 auto 30px;
            text-align: center;
        }
        .header h1 { color: #4a7c8c; font-size: 1.8rem; margin-bottom: 10px; }
        .header p { color: #888; font-size: 0.9rem; }
        .format-group {
            max-width: 800px;
            margin: 0 auto 40px;
        }
        .format-title {
            color: #6a9cac;
            font-size: 1.3rem;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid #2a3f5f;
        }
        .prompt-card {
            background: #16213e;
            border-radius: 12px;
            overflow: hidden;
            border: 1px solid #2a3f5f;
            margin-bottom: 20px;
        }
        .prompt-header {
            background: #0f0f1a;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .prompt-title { color: #4a7c8c; font-weight: 600; }
        .prompt-type {
            background: #4a7c8c;
            color: #fff;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.75rem;
        }
        .prompt-body { padding: 20px; }
        .prompt-text {
            background: #0a0a15;
            padding: 15px;
            border-radius: 8px;
            font-family: 'SF Mono', Monaco, monospace;
            font-size: 0.85rem;
            color: #ccc;
            white-space: pre-wrap;
            word-break: break-word;
            margin-bottom: 15px;
            max-height: 300px;
            overflow-y: auto;
        }
        .copy-btn {
            width: 100%;
            padding: 16px 20px;
            background: #4a7c8c;
            color: #fff;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
        }
        .copy-btn:hover { background: #5a8c9c; }
        .copy-btn.copied { background: #2ecc71; }
        @media (max-width: 600px) {
            body { padding: 15px; }
            .prompt-text { font-size: 0.8rem; }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>[Product] Ad Concepts</h1>
        <p>Generated [Date] — 5 formats, 3 variations each</p>
    </div>

    <!-- FORMAT GROUP 1 -->
    <div class="format-group">
        <h2 class="format-title">Format 1: [Format Name]</h2>

        <div class="prompt-card">
            <div class="prompt-header">
                <span class="prompt-title">1A. [Brief description]</span>
                <span class="prompt-type">[base-ad-id]</span>
            </div>
            <div class="prompt-body">
                <div class="prompt-text" id="prompt-1a">{JSON PROMPT}</div>
                <button class="copy-btn" onclick="copyPrompt('prompt-1a', this)">Copy Prompt</button>
            </div>
        </div>

        <div class="prompt-card">
            <div class="prompt-header">
                <span class="prompt-title">1B. [Brief description]</span>
                <span class="prompt-type">[base-ad-id]</span>
            </div>
            <div class="prompt-body">
                <div class="prompt-text" id="prompt-1b">{JSON PROMPT}</div>
                <button class="copy-btn" onclick="copyPrompt('prompt-1b', this)">Copy Prompt</button>
            </div>
        </div>

        <div class="prompt-card">
            <div class="prompt-header">
                <span class="prompt-title">1C. [Brief description]</span>
                <span class="prompt-type">[base-ad-id]</span>
            </div>
            <div class="prompt-body">
                <div class="prompt-text" id="prompt-1c">{JSON PROMPT}</div>
                <button class="copy-btn" onclick="copyPrompt('prompt-1c', this)">Copy Prompt</button>
            </div>
        </div>
    </div>

    <!-- REPEAT FOR FORMATS 2-5 -->

    <script>
        function copyPrompt(elementId, button) {
            var text = document.getElementById(elementId).textContent;
            navigator.clipboard.writeText(text).then(function() {
                button.textContent = '✓ Copied!';
                button.classList.add('copied');
                setTimeout(function() {
                    button.textContent = 'Copy Prompt';
                    button.classList.remove('copied');
                }, 2000);
            });
        }
    </script>
</body>
</html>
```

## Notes

- Replace `[Product]`, `[Date]`, `[Format Name]`, `[Brief description]`, `[base-ad-id]` with actual values
- Create 5 format groups, 3 prompt cards each
- Increment IDs: prompt-1a, prompt-1b, prompt-1c, prompt-2a, etc.
- File browser URL: `http://100.109.172.85:8080/[path-to-html]`
