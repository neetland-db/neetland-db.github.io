 (cd "$(git rev-parse --show-toplevel)" && git apply --3way <<'EOF' 
diff --git a/index.html b/index.html
new file mode 100644
index 0000000000000000000000000000000000000000..cf2dea14d41c52fc898e5365e6da2c7149672f34
--- /dev/null
+++ b/index.html
@@ -0,0 +1,338 @@
+<!DOCTYPE html>
+<html lang="en">
+  <head>
+    <meta charset="UTF-8" />
+    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
+    <title>NEETland Social Link Database</title>
+    <style>
+      :root {
+        --bg: #0a0f1d;
+        --panel: #111a30;
+        --panel-alt: #152241;
+        --line: #2a406f;
+        --text: #eef4ff;
+        --muted: #9db3dd;
+        --accent: #6fd3ff;
+        --accent-2: #8effb8;
+        --warning: #ffd37f;
+      }
+
+      * {
+        box-sizing: border-box;
+      }
+
+      body {
+        margin: 0;
+        font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
+        background: radial-gradient(circle at top, #16284f 0%, var(--bg) 42%);
+        color: var(--text);
+        min-height: 100vh;
+      }
+
+      .container {
+        width: min(1040px, 92vw);
+        margin: 2rem auto 3rem;
+      }
+
+      .hud {
+        border: 1px solid var(--line);
+        background: linear-gradient(180deg, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
+        border-radius: 14px;
+        overflow: hidden;
+        margin-bottom: 1.25rem;
+      }
+
+      .hud-title {
+        padding: 1rem 1.25rem;
+        display: flex;
+        justify-content: space-between;
+        align-items: center;
+        border-bottom: 1px solid var(--line);
+        background: rgba(7, 12, 25, 0.5);
+      }
+
+      .hud-title h1 {
+        font-size: clamp(1.3rem, 2.8vw, 2rem);
+        margin: 0;
+        letter-spacing: 0.05em;
+        text-transform: uppercase;
+      }
+
+      .status {
+        font-size: 0.85rem;
+        color: var(--warning);
+        letter-spacing: 0.08em;
+      }
+
+      .hud-grid {
+        display: grid;
+        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
+      }
+
+      .cell {
+        padding: 1rem 1.25rem;
+        border-right: 1px solid var(--line);
+      }
+
+      .cell:last-child {
+        border-right: none;
+      }
+
+      .label {
+        display: block;
+        font-size: 0.7rem;
+        text-transform: uppercase;
+        letter-spacing: 0.1em;
+        color: var(--muted);
+        margin-bottom: 0.35rem;
+      }
+
+      .value {
+        font-size: 1rem;
+      }
+
+      .layout {
+        display: grid;
+        gap: 1.2rem;
+        grid-template-columns: 2.15fr 1fr;
+      }
+
+      .panel {
+        border: 1px solid var(--line);
+        border-radius: 14px;
+        background: linear-gradient(180deg, rgba(255, 255, 255, 0.03), rgba(255, 255, 255, 0.01));
+      }
+
+      .panel h2 {
+        margin: 0;
+        font-size: 1rem;
+        letter-spacing: 0.08em;
+        text-transform: uppercase;
+        padding: 0.9rem 1rem;
+        border-bottom: 1px solid var(--line);
+        color: var(--accent);
+      }
+
+      .entries {
+        padding: 0.8rem;
+        display: grid;
+        gap: 0.8rem;
+      }
+
+      .entry {
+        border: 1px solid var(--line);
+        border-radius: 10px;
+        background: var(--panel);
+        padding: 0.9rem;
+      }
+
+      .entry h3 {
+        margin: 0 0 0.55rem;
+        font-size: 1.05rem;
+      }
+
+      .entry p {
+        margin: 0.15rem 0;
+        color: #dce7ff;
+        font-size: 0.95rem;
+        line-height: 1.45;
+      }
+
+      .tags {
+        margin-top: 0.7rem;
+        display: flex;
+        flex-wrap: wrap;
+        gap: 0.4rem;
+      }
+
+      .tag {
+        border: 1px solid #355181;
+        color: var(--muted);
+        padding: 0.2rem 0.55rem;
+        border-radius: 999px;
+        font-size: 0.72rem;
+      }
+
+      .meter {
+        margin-top: 0.7rem;
+      }
+
+      .meter-name {
+        font-size: 0.75rem;
+        text-transform: uppercase;
+        color: var(--muted);
+        margin-bottom: 0.3rem;
+        letter-spacing: 0.08em;
+      }
+
+      .bar {
+        width: 100%;
+        height: 10px;
+        border-radius: 999px;
+        border: 1px solid var(--line);
+        background: #0f172f;
+        overflow: hidden;
+      }
+
+      .bar > span {
+        display: block;
+        height: 100%;
+        border-right: 1px solid rgba(255, 255, 255, 0.2);
+        background: linear-gradient(90deg, var(--accent), var(--accent-2));
+      }
+
+      .side-body {
+        padding: 0.9rem;
+      }
+
+      .log {
+        list-style: none;
+        margin: 0;
+        padding: 0;
+        display: grid;
+        gap: 0.55rem;
+      }
+
+      .log li {
+        border: 1px solid var(--line);
+        border-radius: 8px;
+        background: var(--panel-alt);
+        padding: 0.65rem;
+        font-size: 0.87rem;
+        line-height: 1.35;
+      }
+
+      .log small {
+        display: block;
+        color: var(--muted);
+        margin-bottom: 0.25rem;
+        font-size: 0.72rem;
+      }
+
+      footer {
+        text-align: center;
+        color: var(--muted);
+        font-size: 0.78rem;
+        margin-top: 1.1rem;
+      }
+
+      @media (max-width: 860px) {
+        .layout {
+          grid-template-columns: 1fr;
+        }
+      }
+    </style>
+  </head>
+  <body>
+    <main class="container">
+      <section class="hud">
+        <div class="hud-title">
+          <h1>NEETland Social Link Database</h1>
+          <span class="status">CONFIDENTIAL ARCHIVE // RANK 7</span>
+        </div>
+        <div class="hud-grid">
+          <div class="cell">
+            <span class="label">Zone</span>
+            <span class="value">District 04, NEETland</span>
+          </div>
+          <div class="cell">
+            <span class="label">Database Type</span>
+            <span class="value">Persona & Character Registry</span>
+          </div>
+          <div class="cell">
+            <span class="label">Registered Bonds</span>
+            <span class="value">2 Active Social Links</span>
+          </div>
+        </div>
+      </section>
+
+      <section class="layout">
+        <article class="panel">
+          <h2>Character Files</h2>
+          <div class="entries">
+            <section class="entry">
+              <h3>Agent: You (Arcana: Starbound Operator)</h3>
+              <p>
+                Public persona: calm analyst with a deadpan humor shell.<br />
+                Hidden mode: soft protector who notices every small detail your friend misses.
+              </p>
+              <p>
+                Signature vibe: late-night planner, "I got this" energy, and chaotic lore collector.
+              </p>
+              <div class="tags">
+                <span class="tag">Strategist</span>
+                <span class="tag">Sleepless</span>
+                <span class="tag">Loyal to Core</span>
+              </div>
+              <div class="meter">
+                <div class="meter-name">Social Link Resonance</div>
+                <div class="bar"><span style="width: 78%"></span></div>
+              </div>
+            </section>
+
+            <section class="entry">
+              <h3>Agent: Friend (Arcana: Neon Wanderer)</h3>
+              <p>
+                Public persona: loud chaos beacon, meme machine, and accidental philosopher.<br />
+                Hidden mode: sensitive dreamer who remembers old conversations word-for-word.
+              </p>
+              <p>
+                Signature vibe: spontaneous raids, high-emotion honesty, and dramatic one-liners.
+              </p>
+              <div class="tags">
+                <span class="tag">Wildcard</span>
+                <span class="tag">Creative Core</span>
+                <span class="tag">Emotion Signal+</span>
+              </div>
+              <div class="meter">
+                <div class="meter-name">Trust Uplink</div>
+                <div class="bar"><span style="width: 84%"></span></div>
+              </div>
+            </section>
+
+            <section class="entry">
+              <h3>Combined Persona: "Two-Player Orbit"</h3>
+              <p>
+                When synced, you balance each other: one maps the route, one brings the spark.
+                Conflict potential exists, but recovery speed is unusually high.
+              </p>
+              <p><strong>Bond Ability:</strong> "Back-to-Back" — +30 morale during rough weeks.</p>
+              <div class="meter">
+                <div class="meter-name">Current Bond Rank</div>
+                <div class="bar"><span style="width: 70%"></span></div>
+              </div>
+            </section>
+          </div>
+        </article>
+
+        <aside class="panel">
+          <h2>Incident Log</h2>
+          <div class="side-body">
+            <ul class="log">
+              <li>
+                <small>Entry 01 // 2026-01-04</small>
+                First official "this is our lore now" conversation archived.
+              </li>
+              <li>
+                <small>Entry 02 // 2026-01-19</small>
+                Resolved major misunderstanding in under 24 hours. Rank +1.
+              </li>
+              <li>
+                <small>Entry 03 // 2026-02-11</small>
+                Shared persona concepts and assigned each other codenames.
+              </li>
+              <li>
+                <small>Entry 04 // Ongoing</small>
+                Next rank-up condition: complete a co-op creative project.
+              </li>
+            </ul>
+          </div>
+        </aside>
+      </section>
+
+      <footer>
+        NEETland Social Link Bureau // private record for two certified weirdos.
+      </footer>
+    </main>
+  </body>
+</html>
 
EOF
)
