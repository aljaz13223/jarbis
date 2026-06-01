# jarvis
futuristic AI

import { useEffect, useMemo, useRef, useState } from "react";

type Overlay = "news" | "plan" | "logs" | null;
type Panel = "home" | "weather" | "calendar" | "devices" | "news" | "plan";

export default function JarvisDashboard() {
  const [overlay, setOverlay] = useState<Overlay>(null);
  const [panel, setPanel] = useState<Panel>("home");
  const [page, setPage] = useState(0);

  const commands = useMemo(() => ({
    "open news": () => setOverlay("news"),
    "odpri novice": () => setOverlay("news"),

    "close news": () => setOverlay(null),
    "zapri novice": () => setOverlay(null),

    "open plan": () => setOverlay("plan"),
    "odpri plan": () => setOverlay("plan"),

    "close plan": () => setOverlay(null),
    "zapri plan": () => setOverlay(null),

    "open logs": () => setOverlay("logs"),
    "odpri loge": () => setOverlay("logs"),

    "close logs": () => setOverlay(null),
    "zapri loge": () => setOverlay(null),

    "next page": () => setPage((p) => p + 1),
    "naslednja stran": () => setPage((p) => p + 1),

    "previous page": () => setPage((p) => Math.max(0, p - 1)),
    "prejšnja stran": () => setPage((p) => Math.max(0, p - 1)),

    "go to weather": () => setPanel("weather"),
    "pojdi na vreme": () => setPanel("weather"),

    "go to calendar": () => setPanel("calendar"),
    "pojdi na koledar": () => setPanel("calendar"),

    "go to devices": () => setPanel("devices"),
    "pojdi na naprave": () => setPanel("devices"),
  }), []);

  useJarvisVoice(commands);

  return (
    <main className="jarvis-root">
      <JarvisBackground />

      <section className="dashboard">
        <SystemStatus />

        <NewsPanel onOpen={() => setOverlay("news")} />

        <BusinessPlanCard onOpen={() => setOverlay("plan")} />

        <PanelRouter panel={panel} page={page} />
      </section>

      {overlay === "news" && (
        <FullscreenOverlay onClose={() => setOverlay(null)}>
          <NewsFullscreen />
        </FullscreenOverlay>
      )}

      {overlay === "plan" && (
        <FullscreenOverlay onClose={() => setOverlay(null)}>
          <PlanEditor />
        </FullscreenOverlay>
      )}

      {overlay === "logs" && (
        <FullscreenOverlay onClose={() => setOverlay(null)}>
          <LogsView />
        </FullscreenOverlay>
      )}
    </main>
  );
}
function JarvisBackground() {
  return (
    <div className="jarvis-bg" aria-hidden="true">
      <div className="jarvis-grid" />
      <div className="jarvis-reactor">
        <div className="reactor-ring ring-one" />
        <div className="reactor-ring ring-two" />
        <div className="reactor-core" />
      </div>
      <div className="particles" />
    </div>
  );
}

.jarvis-root {
  position: relative;
  min-height: 100dvh;
  overflow: hidden;
  background: #020816;
  color: #d9faff;
}

.jarvis-bg {
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  overflow: hidden;
}

.jarvis-grid {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(0, 225, 255, 0.08) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0, 225, 255, 0.08) 1px, transparent 1px);
  background-size: 80px 80px;
}

.jarvis-reactor {
  position: absolute;
  top: -22vh;
  left: 50%;
  width: min(105vw, 820px);
  aspect-ratio: 1;
  transform: translateX(-50%);
  opacity: 0.75;
}

.reactor-ring {
  position: absolute;
  inset: 0;
  border-radius: 999px;
  border: 2px dotted rgba(0, 225, 255, 0.7);
  animation: rotateForever 28s linear infinite;
  will-change: transform;
}

.ring-two {
  inset: 12%;
  animation-duration: 42s;
  animation-direction: reverse;
  opacity: 0.55;
}

.reactor-core {
  position: absolute;
  inset: 38%;
  border-radius: 999px;
  background: radial-gradient(circle, rgba(0, 225, 255, 0.8), transparent 65%);
  animation: pulseForever 4s ease-in-out infinite;
}

@keyframes rotateForever {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@keyframes pulseForever {
  0%, 100% {
    opacity: 0.45;
    transform: scale(0.95);
  }
  50% {
    opacity: 0.9;
    transform: scale(1.05);
  }
}

.dashboard {
  position: relative;
  z-index: 1;
  min-height: 100dvh;
  padding: 24px;
}

const news = [
  {
    title: "AI startup update",
    source: "YouTube",
    url: "https://youtube.com/watch?v=example",
  },
  {
    title: "Market news",
    source: "Article",
    url: "https://example.com/news",
  },
  {
    title: "Tech trend",
    source: "Article",
    url: "https://example.com/tech",
  },
];

function NewsPanel({ onOpen }: { onOpen: () => void }) {
  return (
    <section className="panel news-panel" onClick={onOpen}>
      <div className="panel-header">
        <h2>News</h2>
        <span>Open</span>
      </div>

      {news.slice(0, 3).map((item) => (
        <article key={item.url} className="news-preview-item">
          <strong>{item.title}</strong>
          <small>{item.source}</small>
        </article>
      ))}
    </section>
  );
}

function NewsFullscreen() {
  return (
    <div className="fullscreen-grid">
      {news.map((item) => (
        <a
          key={item.url}
          href={item.url}
          target="_blank"
          rel="noreferrer"
          className="news-link-card"
        >
          <strong>{item.title}</strong>
          <span>{item.source}</span>
        </a>
      ))}
    </div>
  );
}

function BusinessPlanCard({ onOpen }: { onOpen: () => void }) {
  return (
    <section className="business-plan-card" onClick={onOpen}>
      <div>
        <p className="label">Business Plan</p>
        <h2>€2,450 / month</h2>
      </div>

      <div className="plan-mini-grid">
        <div>
          <span>Best plan</span>
          <strong>SaaS tools</strong>
        </div>
        <div>
          <span>Next step</span>
          <strong>Landing page</strong>
        </div>
      </div>
    </section>
  );
}

.business-plan-card {
  width: min(100%, 280px);
  aspect-ratio: 16 / 9;
  padding: 14px;
  border: 1px solid rgba(0, 225, 255, 0.35);
  background: rgba(3, 20, 36, 0.72);
  backdrop-filter: blur(14px);
  cursor: pointer;
}

.business-plan-card h2 {
  font-size: 22px;
  margin: 4px 0;
}

.label {
  font-size: 10px;
  letter-spacing: 0.25em;
  color: #00e1ff;
  text-transform: uppercase;
}

.plan-mini-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  font-size: 11px;
}
function FullscreenOverlay({
  children,
  onClose,
}: {
  children: React.ReactNode;
  onClose: () => void;
}) {
  useEffect(() => {
    const close = (e: KeyboardEvent) => {
      if (e.key === "Escape") onClose();
    };

    window.addEventListener("keydown", close);
    return () => window.removeEventListener("keydown", close);
  }, [onClose]);

  return (
    <div className="overlay">
      <button className="overlay-close" onClick={onClose}>
        Close
      </button>

      <div className="overlay-content">
        {children}
      </div>
    </div>
  );
}

.overlay {
  position: fixed;
  inset: 0;
  z-index: 20;
  background: rgba(1, 8, 18, 0.92);
  backdrop-filter: blur(18px);
  padding: 24px;
}

.overlay-content {
  height: 100%;
  border: 1px solid rgba(0, 225, 255, 0.35);
  padding: 20px;
  overflow: auto;
}

.overlay-close {
  position: absolute;
  right: 24px;
  top: 24px;
  z-index: 21;
}
function useJarvisVoice(commands: Record<string, () => void>) {
  const commandsRef = useRef(commands);

  useEffect(() => {
    commandsRef.current = commands;
  }, [commands]);

  useEffect(() => {
    const SpeechRecognition =
      window.SpeechRecognition || window.webkitSpeechRecognition;

    if (!SpeechRecognition) return;

    const recognition = new SpeechRecognition();
    recognition.continuous = true;
    recognition.lang = "sl-SI";
    recognition.interimResults = false;

    recognition.onresult = (event: SpeechRecognitionEvent) => {
      const last = event.results[event.results.length - 1];
      const text = last[0].transcript.toLowerCase().trim();

      runCommand(text, commandsRef.current);
    };

    recognition.onerror = () => {
      setTimeout(() => recognition.start(), 800);
    };

    recognition.onend = () => {
      recognition.start();
    };

    recognition.start();

    return () => recognition.stop();
  }, []);
}

function runCommand(
  text: string,
  commands: Record<string, () => void>
) {
  for (const command of Object.keys(commands)) {
    if (text.includes(command)) {
      commands[command]();
      return;
    }
  }
}

function PlanEditor() {
  const [goals, setGoals] = useState([
    "Launch landing page",
    "Post 3 short videos",
    "Build pricing page",
  ]);

  useEffect(() => {
    window.jarvisPlanCommand = (text: string) => {
      const clean = text.toLowerCase();

      if (clean.includes("add") || clean.includes("dodaj")) {
        const goal = clean
          .replace("jarvis", "")
          .replace("add", "")
          .replace("dodaj", "")
          .replace("to my plan", "")
          .replace("v plan", "")
          .trim();

        if (goal) setGoals((old) => [...old, goal]);
      }

      if (clean.includes("remove") || clean.includes("odstrani")) {
        const target = clean
          .replace("remove", "")
          .replace("odstrani", "")
          .trim();

        setGoals((old) =>
          old.filter((goal) => !goal.toLowerCase().includes(target))
        );
      }
    };
  }, []);

  return (
    <div>
      <h1>Plan Editor</h1>

      {goals.map((goal) => (
        <div key={goal} className="goal-row">
          {goal}
        </div>
      ))}
    </div>
  );
}
if (
  text.includes("add") ||
  text.includes("dodaj") ||
  text.includes("remove") ||
  text.includes("odstrani")
) {
  window.jarvisPlanCommand?.(text);
}

declare global {
  interface Window {
    SpeechRecognition: any;
    webkitSpeechRecognition: any;
    jarvisPlanCommand?: (text: string) => void;
  }
}

components/
  JarvisDashboard.tsx
  JarvisBackground.tsx
  NewsPanel.tsx
  BusinessPlanCard.tsx
  FullscreenOverlay.tsx
  PlanEditor.tsx
  useJarvisVoice.ts

styles/
  jarvis.css
