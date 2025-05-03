<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mathematics Blog Editor</title>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            margin: 0;
            padding: 20px;
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            padding: 20px;
        }
        
        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 30px;
            background: linear-gradient(to right, #3498db, #9b59b6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .editor-container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
        }
        
        .editor-area {
            flex: 1;
            min-width: 300px;
        }
        
        .preview-area {
            flex: 1;
            min-width: 300px;
            border: 1px solid #ddd;
            padding: 15px;
            border-radius: 5px;
            background-color: #f9f9f9;
            min-height: 300px;
        }
        
        textarea {
            width: 100%;
            height: 300px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-family: 'Consolas', monospace;
            font-size: 14px;
            resize: vertical;
        }
        
        .toolbar {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 15px;
            padding: 10px;
            background-color: #f0f0f0;
            border-radius: 5px;
        }
        
        .category {
            margin-bottom: 15px;
        }
        
        .category-title {
            font-weight: bold;
            margin-bottom: 8px;
            color: #3498db;
            display: flex;
            align-items: center;
        }
        
        .category-title:before {
            content: "▶";
            margin-right: 5px;
            font-size: 10px;
        }
        
        .category.expanded .category-title:before {
            content: "▼";
        }
        
        .buttons {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
        }
        
        button {
            padding: 8px 12px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 13px;
            transition: all 0.2s;
        }
        
        button:hover {
            background-color: #2980b9;
            transform: translateY(-2px);
        }
        
        .basic-math button { background-color: #2ecc71; }
        .basic-math button:hover { background-color: #27ae60; }
        
        .algebra button { background-color: #e74c3c; }
        .algebra button:hover { background-color: #c0392b; }
        
        .trigonometry button { background-color: #9b59b6; }
        .trigonometry button:hover { background-color: #8e44ad; }
        
        .calculus button { background-color: #f39c12; }
        .calculus button:hover { background-color: #d35400; }
        
        .advanced button { background-color: #1abc9c; }
        .advanced button:hover { background-color: #16a085; }
        
        .formatting button { background-color: #34495e; }
        .formatting button:hover { background-color: #2c3e50; }
        
        .actions {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
        }
        
        .actions button {
            padding: 10px 20px;
            font-size: 16px;
        }
        
        .save-btn { background-color: #2ecc71; }
        .save-btn:hover { background-color: #27ae60; }
        
        .clear-btn { background-color: #e74c3c; }
        .clear-btn:hover { background-color: #c0392b; }
        
        .copy-btn { background-color: #3498db; }
        .copy-btn:hover { background-color: #2980b9; }
        
        .hidden {
            display: none;
        }
        
        .latex-example {
            font-family: monospace;
            background-color: #f0f0f0;
            padding: 2px 5px;
            border-radius: 3px;
            font-size: 12px;
            margin-top: 5px;
        }
        
        .symbol-preview {
            font-size: 18px;
            margin-left: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Mathematics Blog Editor</h1>
        
        <div class="toolbar">
            <div class="category expanded">
                <div class="category-title">Basic Math</div>
                <div class="buttons basic-math">
                    <button onclick="insertAtCursor('\\frac{a}{b}')">Fraction <span class="symbol-preview">a/b</span></button>
                    <button onclick="insertAtCursor('^{}')">Superscript <span class="symbol-preview">x²</span></button>
                    <button onclick="insertAtCursor('_{}')">Subscript <span class="symbol-preview">x₁</span></button>
                    <button onclick="insertAtCursor('\\sqrt{}')">Square Root <span class="symbol-preview">√x</span></button>
                    <button onclick="insertAtCursor('\\sqrt[n]{}')">nth Root <span class="symbol-preview">ⁿ√x</span></button>
                    <button onclick="insertAtCursor('\\times')">Multiply <span class="symbol-preview">×</span></button>
                    <button onclick="insertAtCursor('\\div')">Divide <span class="symbol-preview">÷</span></button>
                    <button onclick="insertAtCursor('\\pm')">Plus/Minus <span class="symbol-preview">±</span></button>
                    <button onclick="insertAtCursor('\\mp')">Minus/Plus <span class="symbol-preview">∓</span></button>
                </div>
            </div>
            
            <div class="category expanded">
                <div class="category-title">Algebra</div>
                <div class="buttons algebra">
                    <button onclick="insertAtCursor('x^{}')">Power <span class="symbol-preview">xⁿ</span></button>
                    <button onclick="insertAtCursor('\\left(\\right)')">Parentheses <span class="symbol-preview">( )</span></button>
                    <button onclick="insertAtCursor('\\left[\\right]')">Brackets <span class="symbol-preview">[ ]</span></button>
                    <button onclick="insertAtCursor('\\left\\{\\right\\}')">Braces <span class="symbol-preview">{ }</span></button>
                    <button onclick="insertAtCursor('\\left|\\right|')">Absolute Value <span class="symbol-preview">|x|</span></button>
                    <button onclick="insertAtCursor('\\sum_{i=1}^{n}')">Summation <span class="symbol-preview">Σ</span></button>
                    <button onclick="insertAtCursor('\\prod_{i=1}^{n}')">Product <span class="symbol-preview">Π</span></button>
                    <button onclick="insertAtCursor('\\lim_{x \\to a}')">Limit <span class="symbol-preview">lim</span></button>
                    <button onclick="insertAtCursor('\\binom{n}{k}')">Binomial <span class="symbol-preview">C(n,k)</span></button>
                </div>
            </div>
            
            <div class="category expanded">
                <div class="category-title">Trigonometry</div>
                <div class="buttons trigonometry">
                    <button onclick="insertAtCursor('\\sin')">sin <span class="symbol-preview">sin</span></button>
                    <button onclick="insertAtCursor('\\cos')">cos <span class="symbol-preview">cos</span></button>
                    <button onclick="insertAtCursor('\\tan')">tan <span class="symbol-preview">tan</span></button>
                    <button onclick="insertAtCursor('\\csc')">csc <span class="symbol-preview">csc</span></button>
                    <button onclick="insertAtCursor('\\sec')">sec <span class="symbol-preview">sec</span></button>
                    <button onclick="insertAtCursor('\\cot')">cot <span class="symbol-preview">cot</span></button>
                    <button onclick="insertAtCursor('\\arcsin')">arcsin <span class="symbol-preview">sin⁻¹</span></button>
                    <button onclick="insertAtCursor('\\arccos')">arccos <span class="symbol-preview">cos⁻¹</span></button>
                    <button onclick="insertAtCursor('\\arctan')">arctan <span class="symbol-preview">tan⁻¹</span></button>
                    <button onclick="insertAtCursor('\\pi')">pi <span class="symbol-preview">π</span></button>
                    <button onclick="insertAtCursor('\\theta')">theta <span class="symbol-preview">θ</span></button>
                    <button onclick="insertAtCursor('\\degree')">degree <span class="symbol-preview">°</span></button>
                </div>
            </div>
            
            <div class="category expanded">
                <div class="category-title">Calculus</div>
                <div class="buttons calculus">
                    <button onclick="insertAtCursor('\\frac{d}{dx}')">Derivative <span class="symbol-preview">d/dx</span></button>
                    <button onclick="insertAtCursor('\\frac{d^2}{dx^2}')">Second Derivative <span class="symbol-preview">d²/dx²</span></button>
                    <button onclick="insertAtCursor('\\frac{\\partial}{\\partial x}')">Partial Derivative <span class="symbol-preview">∂/∂x</span></button>
                    <button onclick="insertAtCursor('\\int')">Integral <span class="symbol-preview">∫</span></button>
                    <button onclick="insertAtCursor('\\int_{a}^{b}')">Definite Integral <span class="symbol-preview">∫ₐᵇ</span></button>
                    <button onclick="insertAtCursor('\\iint')">Double Integral <span class="symbol-preview">∬</span></button>
                    <button onclick="insertAtCursor('\\iiint')">Triple Integral <span class="symbol-preview">∭</span></button>
                    <button onclick="insertAtCursor('\\oint')">Contour Integral <span class="symbol-preview">∮</span></button>
                    <button onclick="insertAtCursor('\\nabla')">Nabla <span class="symbol-preview">∇</span></button>
                    <button onclick="insertAtCursor('\\Delta')">Delta <span class="symbol-preview">Δ</span></button>
                </div>
            </div>
            
            <div class="category expanded">
                <div class="category-title">Advanced Math</div>
                <div class="buttons advanced">
                    <button onclick="insertAtCursor('\\begin{pmatrix} a & b \\\\ c & d \\end{pmatrix}')">Matrix <span class="symbol-preview">[a b; c d]</span></button>
                    <button onclick="insertAtCursor('\\begin{vmatrix} a & b \\\\ c & d \\end{vmatrix}')">Determinant <span class="symbol-preview">|A|</span></button>
                    <button onclick="insertAtCursor('\\vec{v}')">Vector <span class="symbol-preview">v⃗</span></button>
                    <button onclick="insertAtCursor('\\mathbb{R}')">Real Numbers <span class="symbol-preview">ℝ</span></button>
                    <button onclick="insertAtCursor('\\mathbb{C}')">Complex Numbers <span class="symbol-preview">ℂ</span></button>
                    <button onclick="insertAtCursor('\\mathbb{Z}')">Integers <span class="symbol-preview">ℤ</span></button>
                    <button onclick="insertAtCursor('\\mathbb{N}')">Natural Numbers <span class="symbol-preview">ℕ</span></button>
                    <button onclick="insertAtCursor('\\mathbb{Q}')">Rational Numbers <span class="symbol-preview">ℚ</span></button>
                    <button onclick="insertAtCursor('\\emptyset')">Empty Set <span class="symbol-preview">∅</span></button>
                    <button onclick="insertAtCursor('\\infty')">Infinity <span class="symbol-preview">∞</span></button>
                    <button onclick="insertAtCursor('\\forall')">For All <span class="symbol-preview">∀</span></button>
                    <button onclick="insertAtCursor('\\exists')">Exists <span class="symbol-preview">∃</span></button>
                    <button onclick="insertAtCursor('\\in')">Element Of <span class="symbol-preview">∈</span></button>
                    <button onclick="insertAtCursor('\\notin')">Not In <span class="symbol-preview">∉</span></button>
                    <button onclick="insertAtCursor('\\subset')">Subset <span class="symbol-preview">⊂</span></button>
                    <button onclick="insertAtCursor('\\subseteq')">Subset Equal <span class="symbol-preview">⊆</span></button>
                    <button onclick="insertAtCursor('\\cup')">Union <span class="symbol-preview">∪</span></button>
                    <button onclick="insertAtCursor('\\cap')">Intersection <span class="symbol-preview">∩</span></button>
                    <button onclick="insertAtCursor('\\Gamma')">Gamma Function <span class="symbol-preview">Γ</span></button>
                    <button onclick="insertAtCursor('\\Beta')">Beta Function <span class="symbol-preview">B</span></button>
                </div>
            </div>
            
            <div class="category expanded">
                <div class="category-title">Formatting</div>
                <div class="buttons formatting">
                    <button onclick="insertAtCursor('\\text{ }')">Text <span class="symbol-preview">text</span></button>
                    <button onclick="insertAtCursor('\\textbf{ }')">Bold <span class="symbol-preview">bold</span></button>
                    <button onclick="insertAtCursor('\\textit{ }')">Italic <span class="symbol-preview">italic</span></button>
                    <button onclick="insertAtCursor('\\underline{ }')">Underline <span class="symbol-preview">underline</span></button>
                    <button onclick="insertAtCursor('\\overline{ }')">Overline <span class="symbol-preview">x̄</span></button>
                    <button onclick="insertAtCursor('\\hat{}')">Hat <span class="symbol-preview">â</span></button>
                    <button onclick="insertAtCursor('\\tilde{}')">Tilde <span class="symbol-preview">ã</span></button>
                    <button onclick="insertAtCursor('\\dot{}')">Dot <span class="symbol-preview">ȧ</span></button>
                    <button onclick="insertAtCursor('\\ddot{}')">Double Dot <span class="symbol-preview">ä</span></button>
                    <button onclick="insertAtCursor('\\vec{}')">Vector <span class="symbol-preview">v⃗</span></button>
                    <button onclick="insertAtCursor('\\color{red}{ }')">Color <span class="symbol-preview" style="color:red">red</span></button>
                </div>
            </div>
        </div>
        
        <div class="editor-container">
            <div class="editor-area">
                <textarea id="math-editor" placeholder="Write your mathematical expressions here..."></textarea>
                <div class="latex-example">
                    Example: To write a quadratic equation, type: \$ax^2 + bx + c = 0\$
                </div>
            </div>
            <div class="preview-area" id="preview">
                <h3>Preview will appear here</h3>
            </div>
        </div>
        
        <div class="actions">
            <button class="clear-btn" onclick="clearEditor()">Clear Editor</button>
            <button class="copy-btn" onclick="copyToClipboard()">Copy to Clipboard</button>
            <button class="save-btn" onclick="saveContent()">Save Content</button>
        </div>
    </div>

    <script>
        // Function to insert text at cursor position
        function insertAtCursor(text) {
            const textarea = document.getElementById('math-editor');
            const startPos = textarea.selectionStart;
            const endPos = textarea.selectionEnd;
            const cursorPos = startPos;
            const textBefore = textarea.value.substring(0, startPos);
            const textAfter = textarea.value.substring(endPos, textarea.value.length);
            
            textarea.value = textBefore + text + textAfter;
            textarea.selectionStart = cursorPos + text.length;
            textarea.selectionEnd = cursorPos + text.length;
            textarea.focus();
            
            updatePreview();
        }
        
        // Function to update the preview
        function updatePreview() {
            const editorContent = document.getElementById('math-editor').value;
            const previewDiv = document.getElementById('preview');
            
            // Process content to handle both inline and display math
            let processedContent = editorContent
                .replace(/\$\$(.*?)\$\$/g, '\\[$1\\]')  // Convert $$...$$ to \[...\]
                .replace(/\$(.*?)\$/g, '\\($1\\)');    // Convert $...$ to \(...\)
            
            previewDiv.innerHTML = processedContent;
            
            // Tell MathJax to typeset the new content
            if (typeof MathJax !== 'undefined') {
                MathJax.typesetPromise([previewDiv]).catch(err => console.log('Typeset error:', err));
            }
        }
        
        // Function to clear the editor
        function clearEditor() {
            if (confirm('Are you sure you want to clear the editor?')) {
                document.getElementById('math-editor').value = '';
                document.getElementById('preview').innerHTML = '<h3>Preview will appear here</h3>';
            }
        }
        
        // Function to copy content to clipboard
        function copyToClipboard() {
            const textarea = document.getElementById('math-editor');
            textarea.select();
            document.execCommand('copy');
            
            // Show a temporary notification
            const notification = document.createElement('div');
            notification.textContent = 'Copied to clipboard!';
            notification.style.position = 'fixed';
            notification.style.bottom = '20px';
            notification.style.right = '20px';
            notification.style.backgroundColor = '#2ecc71';
            notification.style.color = 'white';
            notification.style.padding = '10px 20px';
            notification.style.borderRadius = '5px';
            notification.style.zIndex = '1000';
            document.body.appendChild(notification);
            
            setTimeout(() => {
                document.body.removeChild(notification);
            }, 2000);
        }
        
        // Function to save content (simulated for this example)
        function saveContent() {
            const content = document.getElementById('math-editor').value;
            // In a real implementation, you would send this to your server
            alert('Content saved (simulated). In a real implementation, this would save to your blog.');
            console.log('Content to save:', content);
        }
        
        // Initialize event listeners
        document.addEventListener('DOMContentLoaded', function() {
            const textarea = document.getElementById('math-editor');
            textarea.addEventListener('input', updatePreview);
            
            // Add click handlers for category titles
            const categoryTitles = document.querySelectorAll('.category-title');
            categoryTitles.forEach(title => {
                title.addEventListener('click', function() {
                    const category = this.parentElement;
                    const buttons = this.nextElementSibling;
                    
                    if (buttons.classList.contains('hidden')) {
                        buttons.classList.remove('hidden');
                        category.classList.add('expanded');
                    } else {
                        buttons.classList.add('hidden');
                        category.classList.remove('expanded');
                    }
                });
            });
        });
    </script>
</body>
</html>
