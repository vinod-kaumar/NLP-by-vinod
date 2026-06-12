<div style="font-family:Arial, sans-serif; line-height:1.6; color:#1f2937; max-width:1100px; margin:auto;">

<div style="background:linear-gradient(135deg,#7c3aed,#2563eb,#06b6d4); border-radius:18px; padding:24px; text-align:center; color:white; margin-bottom:22px; box-shadow:0 10px 25px rgba(37,99,235,.22);">
  <h1 style="margin:0; font-size:34px; letter-spacing:.5px;">LSTM — Long Short-Term Memory</h1>
  <p style="margin:8px 0 0; font-size:15px; opacity:.92;">A gated RNN architecture for remembering long-term context in sequential data</p>
</div>

<div style="background:#f8fafc; border:1px solid #e5e7eb; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#4c1d95;">1. Core Definition</h2>
  <div style="background:#f5f3ff; border-left:6px solid #7c3aed; border-radius:12px; padding:14px;">
    <b>LSTM</b> stands for <b>Long Short-Term Memory</b>. It is an improved version of RNN designed to handle long sequences by using a special memory mechanism.
  </div>

  <table style="width:100%; border-collapse:collapse; margin-top:16px; font-size:14px;">
    <tr style="background:#111827; color:white;">
      <th style="padding:10px; text-align:left;">Idea</th>
      <th style="padding:10px; text-align:left;">Meaning</th>
    </tr>
    <tr>
      <td style="padding:10px; border:1px solid #e5e7eb;"><b>Sequential model</b></td>
      <td style="padding:10px; border:1px solid #e5e7eb;">Processes input step by step: word by word or time step by time step.</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:10px; border:1px solid #e5e7eb;"><b>Gated memory</b></td>
      <td style="padding:10px; border:1px solid #e5e7eb;">Uses gates to decide what to forget, what to store, and what to output.</td>
    </tr>
    <tr>
      <td style="padding:10px; border:1px solid #e5e7eb;"><b>Main benefit</b></td>
      <td style="padding:10px; border:1px solid #e5e7eb;">Handles long-term dependencies better than simple RNN.</td>
    </tr>
  </table>
</div>

<div style="background:#fff1f2; border:1px solid #fda4af; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#be123c;">2. Why LSTM was needed</h2>
  <p>Simple RNNs struggle when useful information is far behind in the sequence.</p>

  <div style="display:flex; gap:12px; flex-wrap:wrap;">
    <div style="flex:1; min-width:250px; background:white; border:1px solid #fecdd3; border-radius:14px; padding:14px;">
      <h3 style="margin-top:0; color:#be123c;">Problems in RNN</h3>
      <ul style="margin-bottom:0;">
        <li>Long-term dependency problem</li>
        <li>Vanishing gradient problem</li>
        <li>Exploding gradient problem</li>
        <li>Old context can be overwritten</li>
        <li>Long sequences become difficult to remember</li>
      </ul>
    </div>

    <div style="flex:1; min-width:250px; background:white; border:1px solid #fed7aa; border-radius:14px; padding:14px;">
      <h3 style="margin-top:0; color:#9a3412;">Reason</h3>
      <ul style="margin-bottom:0;">
        <li>Simple RNN mainly carries context through one hidden state.</li>
        <li>The same path handles both recent and old information.</li>
        <li>As the sequence grows, earlier information becomes weak.</li>
      </ul>
    </div>
  </div>

  <div style="margin-top:14px; background:#fff7ed; border-left:6px solid #ea580c; border-radius:12px; padding:13px;">
    <b>Correction:</b> LSTM does not have “two hidden states.” It has one <b>cell state</b> and one <b>hidden state</b>.
  </div>
</div>

<div style="background:#eff6ff; border:1px solid #93c5fd; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#1d4ed8;">3. Intuition with example</h2>
  <div style="background:#0f172a; color:#e5e7eb; border-radius:12px; padding:14px; font-family:Consolas, monospace; overflow-x:auto;">
    Vinod is a student of B.Tech final year. He studies at VNIT Nagpur.
  </div>
  <p>To understand <b>“He”</b>, the model must remember earlier context: <b>Vinod</b>. This is long-term dependency.</p>

  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <tr style="background:#1e3a8a; color:white;">
      <th style="padding:10px; text-align:left;">Memory type</th>
      <th style="padding:10px; text-align:left;">Example</th>
      <th style="padding:10px; text-align:left;">In LSTM</th>
    </tr>
    <tr>
      <td style="padding:10px; border:1px solid #bfdbfe;"><b>Short-term context</b></td>
      <td style="padding:10px; border:1px solid #bfdbfe;">Current phrase: “He studies”</td>
      <td style="padding:10px; border:1px solid #bfdbfe;">Hidden state h<sub>t</sub></td>
    </tr>
    <tr style="background:#f8fafc;">
      <td style="padding:10px; border:1px solid #bfdbfe;"><b>Long-term context</b></td>
      <td style="padding:10px; border:1px solid #bfdbfe;">“He” refers to Vinod</td>
      <td style="padding:10px; border:1px solid #bfdbfe;">Cell state C<sub>t</sub></td>
    </tr>
  </table>
</div>

<div style="background:#f8fafc; border:1px solid #cbd5e1; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#0f172a;">4. LSTM Architecture</h2>

  <div style="background:linear-gradient(135deg,#eef2ff,#f8fafc); border:2px solid #c4b5fd; border-radius:18px; padding:18px;">
    <h3 style="text-align:center; color:#4338ca; margin-top:0;">One LSTM Cell at Time Step t</h3>

    <div style="text-align:center; margin:14px 0;">
      <span style="display:inline-block; background:#dbeafe; color:#1e3a8a; border:1px solid #3b82f6; border-radius:12px; padding:10px 14px; margin:5px;"><b>x<sub>t</sub></b><br>Current Input</span>
      <span style="font-size:24px; font-weight:bold; color:#475569;">+</span>
      <span style="display:inline-block; background:#dcfce7; color:#14532d; border:1px solid #22c55e; border-radius:12px; padding:10px 14px; margin:5px;"><b>h<sub>t-1</sub></b><br>Previous Hidden</span>
      <span style="font-size:24px; font-weight:bold; color:#475569;">+</span>
      <span style="display:inline-block; background:#fef3c7; color:#78350f; border:1px solid #f59e0b; border-radius:12px; padding:10px 14px; margin:5px;"><b>C<sub>t-1</sub></b><br>Previous Cell</span>
    </div>

    <div style="text-align:center; font-size:28px; color:#475569;">↓</div>

    <div style="text-align:center; margin:14px 0;">
      <span style="display:inline-block; background:#ffe4e6; color:#be123c; border:1px solid #fb7185; border-radius:12px; padding:10px 14px; margin:5px;"><b>Forget Gate</b><br>f<sub>t</sub></span>
      <span style="font-size:24px; font-weight:bold; color:#475569;">→</span>
      <span style="display:inline-block; background:#dcfce7; color:#166534; border:1px solid #4ade80; border-radius:12px; padding:10px 14px; margin:5px;"><b>Input Gate</b><br>i<sub>t</sub>, C̃<sub>t</sub></span>
      <span style="font-size:24px; font-weight:bold; color:#475569;">→</span>
      <span style="display:inline-block; background:#dbeafe; color:#1d4ed8; border:1px solid #60a5fa; border-radius:12px; padding:10px 14px; margin:5px;"><b>Output Gate</b><br>o<sub>t</sub></span>
    </div>

    <div style="background:linear-gradient(90deg,#fef3c7,#fde68a,#fef3c7); border:2px solid #f59e0b; color:#78350f; border-radius:14px; padding:12px; text-align:center; font-weight:bold; margin:14px auto; max-width:760px;">
      Cell State Highway: C<sub>t-1</sub> → C<sub>t</sub><br>
      This path carries long-term memory forward.
    </div>

    <div style="text-align:center; margin:14px 0;">
      <span style="display:inline-block; background:#fef3c7; color:#78350f; border:1px solid #f59e0b; border-radius:12px; padding:10px 14px; margin:5px;"><b>C<sub>t</sub></b><br>Current Cell State</span>
      <span style="font-size:24px; font-weight:bold; color:#475569;">+</span>
      <span style="display:inline-block; background:#e0f2fe; color:#075985; border:1px solid #38bdf8; border-radius:12px; padding:10px 14px; margin:5px;"><b>h<sub>t</sub></b><br>Current Hidden State</span>
    </div>
  </div>
</div>

<div style="background:#f0fdf4; border:1px solid #86efac; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#166534;">5. Cell State vs Hidden State</h2>
  <div style="display:flex; gap:12px; flex-wrap:wrap;">
    <div style="flex:1; min-width:250px; background:white; border:1px solid #fde68a; border-radius:14px; padding:14px;">
      <h3 style="margin-top:0; color:#854d0e;">Cell State — C<sub>t</sub></h3>
      <ul style="margin-bottom:0;">
        <li>Acts like long-term memory.</li>
        <li>Carries useful information across many time steps.</li>
        <li>Provides a smoother path for gradient flow.</li>
        <li>Helps reduce vanishing gradient problem.</li>
      </ul>
    </div>
    <div style="flex:1; min-width:250px; background:white; border:1px solid #bfdbfe; border-radius:14px; padding:14px;">
      <h3 style="margin-top:0; color:#1d4ed8;">Hidden State — h<sub>t</sub></h3>
      <ul style="margin-bottom:0;">
        <li>Acts like short-term/current context.</li>
        <li>Represents current output.</li>
        <li>Passed to the next time step.</li>
        <li>Used for prediction or next sequence processing.</li>
      </ul>
    </div>
  </div>
</div>

<div style="background:#fff7ed; border:1px solid #fed7aa; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#9a3412;">6. The Three Gates</h2>

  <div style="display:flex; gap:12px; flex-wrap:wrap;">
    <div style="flex:1; min-width:240px; background:#fff1f2; border:1px solid #fb7185; border-radius:14px; padding:14px;">
      <h3 style="margin-top:0; color:#be123c;">1. Forget Gate</h3>
      <p><b>Question:</b> What old information should I forget?</p>
      <p><b>Purpose:</b> Removes useless information from old cell state.</p>
      <code>f<sub>t</sub> = σ(W<sub>f</sub>[h<sub>t-1</sub>, x<sub>t</sub>] + b<sub>f</sub>)</code>
    </div>

    <div style="flex:1; min-width:240px; background:#f0fdf4; border:1px solid #4ade80; border-radius:14px; padding:14px;">
      <h3 style="margin-top:0; color:#166534;">2. Input Gate</h3>
      <p><b>Question:</b> What new information should I store?</p>
      <p><b>Purpose:</b> Adds useful new memory into cell state.</p>
      <code>i<sub>t</sub> = σ(W<sub>i</sub>[h<sub>t-1</sub>, x<sub>t</sub>] + b<sub>i</sub>)</code><br>
      <code>C̃<sub>t</sub> = tanh(W<sub>c</sub>[h<sub>t-1</sub>, x<sub>t</sub>] + b<sub>c</sub>)</code>
    </div>

    <div style="flex:1; min-width:240px; background:#eff6ff; border:1px solid #60a5fa; border-radius:14px; padding:14px;">
      <h3 style="margin-top:0; color:#1d4ed8;">3. Output Gate</h3>
      <p><b>Question:</b> What should I output now?</p>
      <p><b>Purpose:</b> Creates current hidden state.</p>
      <code>o<sub>t</sub> = σ(W<sub>o</sub>[h<sub>t-1</sub>, x<sub>t</sub>] + b<sub>o</sub>)</code><br>
      <code>h<sub>t</sub> = o<sub>t</sub> * tanh(C<sub>t</sub>)</code>
    </div>
  </div>
</div>

<div style="background:#f8fafc; border:1px solid #cbd5e1; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#0f172a;">7. Cell State Update</h2>

  <div style="background:linear-gradient(135deg,#fff7ed,#f8fafc); border:2px solid #fed7aa; border-radius:18px; padding:16px;">
    <h3 style="text-align:center; color:#9a3412; margin-top:0;">How LSTM updates long-term memory</h3>

    <div style="text-align:center; margin:12px 0;">
      <span style="display:inline-block; background:#fef3c7; border:1px solid #f59e0b; color:#78350f; border-radius:12px; padding:10px 14px; margin:5px;">Old Memory<br><b>C<sub>t-1</sub></b></span>
      <span style="font-size:22px; font-weight:bold;">×</span>
      <span style="display:inline-block; background:#ffe4e6; border:1px solid #fb7185; color:#be123c; border-radius:12px; padding:10px 14px; margin:5px;">Forget Gate<br><b>f<sub>t</sub></b></span>
      <span style="font-size:22px; font-weight:bold;">=</span>
      <span style="display:inline-block; background:#e2e8f0; border:1px solid #64748b; color:#0f172a; border-radius:12px; padding:10px 14px; margin:5px;">Kept Old Memory</span>
    </div>

    <div style="text-align:center; margin:12px 0;">
      <span style="display:inline-block; background:#dcfce7; border:1px solid #4ade80; color:#166534; border-radius:12px; padding:10px 14px; margin:5px;">Input Gate<br><b>i<sub>t</sub></b></span>
      <span style="font-size:22px; font-weight:bold;">×</span>
      <span style="display:inline-block; background:#ede9fe; border:1px solid #a78bfa; color:#5b21b6; border-radius:12px; padding:10px 14px; margin:5px;">Candidate Memory<br><b>C̃<sub>t</sub></b></span>
      <span style="font-size:22px; font-weight:bold;">=</span>
      <span style="display:inline-block; background:#e2e8f0; border:1px solid #64748b; color:#0f172a; border-radius:12px; padding:10px 14px; margin:5px;">New Useful Memory</span>
    </div>

    <div style="text-align:center; margin-top:16px; background:#0f172a; color:white; border-radius:12px; padding:14px; font-family:Consolas, monospace;">
      C<sub>t</sub> = f<sub>t</sub> * C<sub>t-1</sub> + i<sub>t</sub> * C̃<sub>t</sub>
    </div>
  </div>
</div>

<div style="background:#f5f3ff; border:1px solid #c4b5fd; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#5b21b6;">8. Complete LSTM Flow</h2>

  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <tr style="background:#4c1d95; color:white;">
      <th style="padding:10px; text-align:left;">Step</th>
      <th style="padding:10px; text-align:left;">Operation</th>
      <th style="padding:10px; text-align:left;">Meaning</th>
    </tr>
    <tr>
      <td style="padding:10px; border:1px solid #ddd6fe;"><b>1</b></td>
      <td style="padding:10px; border:1px solid #ddd6fe;">Forget gate</td>
      <td style="padding:10px; border:1px solid #ddd6fe;">Remove useless old memory.</td>
    </tr>
    <tr style="background:#faf5ff;">
      <td style="padding:10px; border:1px solid #ddd6fe;"><b>2</b></td>
      <td style="padding:10px; border:1px solid #ddd6fe;">Input gate</td>
      <td style="padding:10px; border:1px solid #ddd6fe;">Decide what new information to store.</td>
    </tr>
    <tr>
      <td style="padding:10px; border:1px solid #ddd6fe;"><b>3</b></td>
      <td style="padding:10px; border:1px solid #ddd6fe;">Cell state update</td>
      <td style="padding:10px; border:1px solid #ddd6fe;">Combine useful old memory and useful new memory.</td>
    </tr>
    <tr style="background:#faf5ff;">
      <td style="padding:10px; border:1px solid #ddd6fe;"><b>4</b></td>
      <td style="padding:10px; border:1px solid #ddd6fe;">Output gate</td>
      <td style="padding:10px; border:1px solid #ddd6fe;">Generate current hidden state.</td>
    </tr>
  </table>
</div>

<div style="background:#ecfeff; border:1px solid #67e8f9; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#0e7490;">9. RNN vs LSTM</h2>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <tr style="background:#0e7490; color:white;">
      <th style="padding:10px; text-align:left;">Feature</th>
      <th style="padding:10px; text-align:left;">Simple RNN</th>
      <th style="padding:10px; text-align:left;">LSTM</th>
    </tr>
    <tr>
      <td style="padding:10px; border:1px solid #a5f3fc;">Memory</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">Mainly hidden state</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">Cell state + hidden state</td>
    </tr>
    <tr style="background:#f0fdfa;">
      <td style="padding:10px; border:1px solid #a5f3fc;">Long-term dependency</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">Weak</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">Strong</td>
    </tr>
    <tr>
      <td style="padding:10px; border:1px solid #a5f3fc;">Vanishing gradient</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">High chance</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">Reduced</td>
    </tr>
    <tr style="background:#f0fdfa;">
      <td style="padding:10px; border:1px solid #a5f3fc;">Architecture</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">Simple</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">More complex</td>
    </tr>
    <tr>
      <td style="padding:10px; border:1px solid #a5f3fc;">Gates</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">No gates</td>
      <td style="padding:10px; border:1px solid #a5f3fc;">Forget, Input, Output gates</td>
    </tr>
  </table>
</div>

<div style="background:#f8fafc; border:1px solid #cbd5e1; border-radius:16px; padding:18px; margin-bottom:18px;">
  <h2 style="margin-top:0; color:#0f172a;">10. Final Summary</h2>
  <div style="margin-bottom:12px;">
    <span style="display:inline-block; background:#ede9fe; color:#5b21b6; border-radius:999px; padding:5px 10px; font-size:13px; font-weight:bold; margin:3px;">Sequential Model</span>
    <span style="display:inline-block; background:#dbeafe; color:#1e40af; border-radius:999px; padding:5px 10px; font-size:13px; font-weight:bold; margin:3px;">Gated RNN</span>
    <span style="display:inline-block; background:#dcfce7; color:#166534; border-radius:999px; padding:5px 10px; font-size:13px; font-weight:bold; margin:3px;">Long-Term Memory</span>
    <span style="display:inline-block; background:#ffe4e6; color:#be123c; border-radius:999px; padding:5px 10px; font-size:13px; font-weight:bold; margin:3px;">Forget Gate</span>
    <span style="display:inline-block; background:#ffedd5; color:#9a3412; border-radius:999px; padding:5px 10px; font-size:13px; font-weight:bold; margin:3px;">Input Gate</span>
    <span style="display:inline-block; background:#e0f2fe; color:#075985; border-radius:999px; padding:5px 10px; font-size:13px; font-weight:bold; margin:3px;">Output Gate</span>
  </div>

  <div style="background:#f5f3ff; border-left:6px solid #7c3aed; border-radius:12px; padding:14px;">
    <b>Best one-line definition:</b><br>
    LSTM is a sequential neural network architecture that improves RNN by using gated memory cells to preserve long-term context and handle long sequences more effectively.
  </div>
</div>

</div>
