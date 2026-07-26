import { useState, useEffect, useRef } from "react";

/* ============================================================
   TRUTH TELLER — KNOW YOUR RIGHTS
   Design concept: a case-file dossier. Manila folder tabs across
   the top (one per state), a stamped document underneath. Locked
   states carry a wax-seal padlock stamp until an email unlocks
   the file. Georgia ships open as the free preview.
   ============================================================ */

const INK = "#211F1C";
const INK_FAINT = "#6B6459";
const PAPER = "#E8E1D0";
const PAPER_DARK = "#DBD2BC";
const RED = "#A6192E";
const RED_DARK = "#7A1122";

const FONTS = `
@import url('https://fonts.googleapis.com/css2?family=PT+Serif:ital,wght@0,400;0,700;1,400&family=IBM+Plex+Mono:wght@400;500;600&display=swap');
`;

const STATES = [
  { code: "GA", name: "Georgia", free: true },
  { code: "FL", name: "Florida", free: false },
  { code: "AL", name: "Alabama", free: false },
  { code: "LA", name: "Louisiana", free: false },
  { code: "SC", name: "South Carolina", free: false },
  { code: "TX", name: "Texas", free: false },
  { code: "NC", name: "North Carolina", free: false },
];

// ---- content ----------------------------------------------------
const DATA = {
  GA: {
    stamp: "CLEARED FOR PREVIEW",
    scenarios: [
      {
        title: "Traffic Stop",
        lines: [
          { body: "Pull over safely, cut the engine, hands on the wheel, window down." },
          { label: "What to say when they ask to search:" },
          { script: "Officer, I do not consent to any searches." },
          { body: "If they ask you to step out, comply — that's a lawful order. Staying calm is strategy, not weakness." },
          { body: "Field sobriety tests are voluntary. But Georgia's implied consent law means refusing a chemical breath, blood, or urine test after a DUI arrest carries its own license consequences." },
          { label: "Once the stop's business is done:" },
          { script: "Am I free to go?" },
        ],
      },
      {
        title: "Street Stop",
        lines: [
          { script: "Am I being detained, or am I free to go?" },
          { label: "If detained:" },
          { script: "I am exercising my right to remain silent. I do not consent to a search." },
          { body: "If they attempt a pat-down for a suspected weapon, don't resist it — but state clearly you haven't consented to anything beyond that." },
        ],
      },
      {
        title: "Home Visit",
        lines: [
          { script: "I do not consent to a search. Do you have a warrant?" },
          { body: "If they have one, ask to see it before anyone enters — officers may only search what it names. If they don't, speak through a cracked door or step outside and close it behind you." },
        ],
      },
    ],
  },
  FL: {
    stamp: "RESTRICTED",
    scenarios: [
      {
        title: "Traffic Stop",
        lines: [
          { script: "Officer, I do not consent to any searches." },
          { body: "License, registration, and proof of insurance are required on request." },
          { script: "Am I free to go?" },
        ],
      },
      {
        title: "Street Stop",
        lines: [
          { body: "Under the Stop and Frisk Law, an officer with reasonable suspicion can briefly detain you and ask your identity and purpose; a pat-down is allowed if a weapon is suspected." },
          { script: "Am I being detained, or am I free to go?" },
          { label: "If detained:" },
          { script: "I am exercising my right to remain silent beyond identifying myself. I do not consent to a search." },
        ],
      },
      {
        title: "Home Visit",
        lines: [
          { script: "I do not consent to a search. Do you have a warrant?" },
          { body: "If they have one, ask to see it first. If not, speak through a cracked door or step outside and close it behind you." },
        ],
      },
      {
        title: "Recording",
        lines: [
          { script: "I'm allowed to record this. You're performing a public duty in public." },
          { note: "Video is solidly protected — officers on public duty in public have no expectation of privacy. Some attorneys advise video-only, since Florida's two-party consent law makes audio a closer call." },
        ],
      },
    ],
  },
  AL: {
    stamp: "RESTRICTED",
    scenarios: [
      {
        title: "Traffic Stop",
        lines: [
          { script: "Officer, I do not consent to any searches." },
          { body: "License and proof of insurance are required on request." },
          { script: "Am I free to go?" },
        ],
      },
      {
        title: "Street Stop",
        lines: [
          { body: "Alabama Code § 15-5-30 lets an officer with reasonable suspicion ask your name, address, and an explanation of your actions during a lawful stop." },
          { script: "Am I being detained, or am I free to go?" },
          { label: "If detained:" },
          { script: "I am exercising my right to remain silent. I do not consent to a search." },
          { note: "The statute has no built-in penalty for silence, but that's not the same as safe. What IS clearly illegal is giving a false name. Stay quiet or state your real name — never make one up." },
        ],
      },
      {
        title: "Home Visit",
        lines: [
          { script: "I do not consent to a search. Do you have a warrant?" },
          { body: "If they have one, ask to see it first. If not, speak through a cracked door or step outside and close it behind you." },
        ],
      },
      {
        title: "Recording",
        lines: [
          { script: "I have the right to record this." },
          { note: "Broad First Amendment right to record police in public, as long as you're not physically interfering." },
        ],
      },
    ],
  },
  LA: {
    stamp: "RESTRICTED",
    scenarios: [
      {
        title: "Traffic Stop",
        lines: [
          { script: "Officer, I do not consent to any searches." },
          { body: "License, registration, and proof of insurance are required on request." },
          { script: "Am I free to go?" },
        ],
      },
      {
        title: "Street Stop",
        lines: [
          { body: "Louisiana CCRP Art. 215.1 lets an officer demand your name, address, and an explanation of your actions during a lawful stop." },
          { script: "Am I being detained, or am I free to go?" },
          { note: "Unlike Alabama, refusing to identify here is treated as obstruction and can lead to real charges. Identify yourself, then go silent." },
          { label: "If detained:" },
          { script: "I am identifying myself as required. Beyond that, I am remaining silent, and I do not consent to a search." },
        ],
      },
      {
        title: "Home Visit",
        lines: [
          { script: "I do not consent to a search. Do you have a warrant?" },
          { body: "Officers cannot enter your home — including porches, decks, and garages — without a warrant, except in genuine emergencies." },
          { body: "If they have one, ask to see it first. If not, speak through a cracked door or step outside and close it behind you." },
        ],
      },
      {
        title: "Recording",
        lines: [
          { script: "I have the right to record this." },
          { note: "Recording audio and video of an officer performing public duties in public is allowed in Louisiana." },
        ],
      },
    ],
  },
  SC: {
    stamp: "RESTRICTED",
    scenarios: [
      {
        title: "Traffic Stop",
        lines: [
          { script: "Officer, I do not consent to any searches." },
          { body: "License, registration, and proof of insurance are required on request." },
          { script: "Am I free to go?" },
        ],
      },
      {
        title: "Street Stop",
        lines: [
          { body: "South Carolina has no stop-and-identify statute." },
          { script: "Am I being detained, or am I free to go?" },
          { label: "If detained:" },
          { script: "I am exercising my right to remain silent. I do not consent to a search." },
        ],
      },
      {
        title: "Home Visit",
        lines: [
          { script: "I do not consent to a search. Do you have a warrant?" },
          { body: "If they have one, ask to see it first. If not, speak through a cracked door or step outside and close it behind you." },
        ],
      },
      {
        title: "Recording",
        lines: [
          { script: "I have the right to record this. I'm a participant in this interaction." },
          { note: "South Carolina is a one-party consent state — since you're a party to your own stop, your own consent is enough to record. Less clear if you're a bystander recording someone else's stop." },
        ],
      },
    ],
  },
  TX: {
    stamp: "RESTRICTED",
    scenarios: [
      {
        title: "Pedestrian",
        lines: [
          { body: "No obligation to identify yourself at all in a casual, consensual encounter." },
          { script: "Am I being detained, or am I free to go?" },
        ],
      },
      {
        title: "Detained",
        lines: [
          { script: "I am exercising my right to remain silent." },
          { note: "Staying silent about your name during a detention is NOT a crime. Giving a FALSE name IS a crime. Silence is safe — a lie is not." },
        ],
      },
      {
        title: "Driver",
        lines: [
          { body: "Provide your driver's license on request — required for a lawfully detained driver." },
          { script: "Officer, I do not consent to any searches." },
          { script: "Am I free to go?" },
        ],
      },
      {
        title: "Passenger",
        lines: [
          { body: "Not required to identify unless the officer has specific reasonable suspicion that you, the passenger, are involved in a crime." },
          { script: "Am I being detained?" },
        ],
      },
      {
        title: "Home Visit",
        lines: [
          { script: "I do not consent to a search. Do you have a warrant?" },
          { body: "If they have one, ask to see it first. If not, speak through a cracked door or step outside and close it behind you." },
        ],
      },
      {
        title: "Recording",
        lines: [
          { script: "I have the right to record this — you're performing a public duty in public." },
          { note: "One of the more settled states here: one-party consent plus a clearly established First Amendment right protects recording your own stop or someone else's, as long as you're not interfering." },
        ],
      },
    ],
  },
  NC: {
    stamp: "RESTRICTED",
    scenarios: [
      {
        title: "Traffic Stop",
        lines: [
          { script: "Officer, I do not consent to any searches." },
          { body: "License, registration, and proof of insurance are required on request." },
          { script: "Am I free to go?" },
        ],
      },
      {
        title: "Street Stop",
        lines: [
          { body: "North Carolina has no stop-and-identify statute." },
          { script: "Am I being detained, or am I free to go?" },
          { label: "If detained:" },
          { script: "I am exercising my right to remain silent. I do not consent to a search." },
        ],
      },
      {
        title: "Home Visit",
        lines: [
          { script: "I do not consent to a search. Do you have a warrant?" },
          { body: "If they have one, ask to see it first. If not, speak through a cracked door or step outside and close it behind you." },
        ],
      },
      {
        title: "Recording",
        lines: [
          { script: "I have the right to record this." },
          { note: "Sharpe v. Winterville Police Department (4th Cir., 2023): a passenger livestreaming his own stop was threatened with device seizure — the court struck that policy down, ruling the First Amendment plausibly protects recording your own stop." },
          { note: "Courts have treated recording someone else's stop as a bystander somewhat differently — that distinction is less settled." },
        ],
      },
    ],
  },
};

// ---- small components --------------------------------------------

function Padlock({ size = 12, color = INK }) {
  return (
    <svg width={size} height={size} viewBox="0 0 24 24" fill="none">
      <rect x="5" y="11" width="14" height="10" rx="1.5" stroke={color} strokeWidth="2" />
      <path d="M8 11V7a4 4 0 0 1 8 0v4" stroke={color} strokeWidth="2" fill="none" />
    </svg>
  );
}

function FolderTab({ st, active, onClick }) {
  return (
    <button
      onClick={onClick}
      style={{
        position: "relative",
        fontFamily: "'IBM Plex Mono', monospace",
        fontSize: 12,
        letterSpacing: "0.06em",
        fontWeight: 600,
        color: active ? PAPER : INK_FAINT,
        background: active ? INK : PAPER_DARK,
        border: "none",
        borderTop: `2px solid ${active ? RED : "transparent"}`,
        padding: "10px 14px 9px",
        borderRadius: "6px 6px 0 0",
        cursor: "pointer",
        display: "flex",
        alignItems: "center",
        gap: 6,
        transform: active ? "translateY(0)" : "translateY(3px)",
        transition: "all 0.15s ease",
        whiteSpace: "nowrap",
        flexShrink: 0,
      }}
    >
      {st.code}
      {!st.free && <Padlock size={10} color={active ? PAPER : INK_FAINT} />}
    </button>
  );
}

function ScriptLine({ text }) {
  return (
    <div
      style={{
        background: "#F4EEE1",
        borderLeft: `4px solid ${RED}`,
        padding: "12px 16px",
        margin: "10px 0",
        fontFamily: "'IBM Plex Mono', monospace",
        fontSize: 15,
        lineHeight: 1.5,
        color: INK,
      }}
    >
      "{text}"
    </div>
  );
}

function EmailGate({ onSubmit, onClose }) {
  const [email, setEmail] = useState("");
  return (
    <div
      style={{
        position: "fixed",
        inset: 0,
        background: "rgba(33,31,28,0.55)",
        display: "flex",
        alignItems: "center",
        justifyContent: "center",
        zIndex: 50,
        padding: 20,
      }}
      onClick={onClose}
    >
      <div
        onClick={(e) => e.stopPropagation()}
        style={{
          background: PAPER,
          maxWidth: 380,
          width: "100%",
          padding: "28px 26px 24px",
          border: `1px solid ${INK}`,
          boxShadow: "6px 6px 0 " + INK,
          position: "relative",
        }}
      >
        <div
          style={{
            position: "absolute",
            top: 14,
            right: 16,
            fontFamily: "'IBM Plex Mono', monospace",
            fontSize: 10,
            letterSpacing: "0.1em",
            color: INK_FAINT,
          }}
        >
          FILE REQUEST
        </div>
        <h3
          style={{
            fontFamily: "'PT Serif', serif",
            fontSize: 22,
            fontWeight: 700,
            color: INK,
            margin: "6px 0 8px",
          }}
        >
          Unlock every state
        </h3>
        <p style={{ color: INK_FAINT, fontSize: 14, lineHeight: 1.5, margin: "0 0 18px" }}>
          Enter your email to open all 7 files and save your rights card for offline access.
        </p>
        <input
          type="email"
          placeholder="you@email.com"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          style={{
            width: "100%",
            boxSizing: "border-box",
            padding: "10px 12px",
            border: `1.5px solid ${INK}`,
            fontFamily: "'IBM Plex Mono', monospace",
            fontSize: 14,
            marginBottom: 14,
            background: "#fff",
          }}
        />
        <button
          onClick={() => email.includes("@") && onSubmit(email)}
          style={{
            width: "100%",
            padding: "12px",
            background: RED,
            color: PAPER,
            border: "none",
            fontFamily: "'IBM Plex Mono', monospace",
            fontWeight: 600,
            fontSize: 13,
            letterSpacing: "0.05em",
            cursor: "pointer",
          }}
        >
          UNLOCK FILES
        </button>
        <button
          onClick={onClose}
          style={{
            width: "100%",
            padding: "8px",
            marginTop: 8,
            background: "none",
            border: "none",
            color: INK_FAINT,
            fontSize: 12,
            cursor: "pointer",
            textDecoration: "underline",
          }}
        >
          Not now
        </button>
      </div>
    </div>
  );
}

export default function RightsApp() {
  const [active, setActive] = useState("GA");
  const [scenarioIdx, setScenarioIdx] = useState(0);
  const [unlocked, setUnlocked] = useState(false);
  const [gateOpen, setGateOpen] = useState(false);
  const [pendingState, setPendingState] = useState(null);
  const [showUpsell, setShowUpsell] = useState(false);
  const viewedOnce = useRef(false);

  useEffect(() => {
    try {
      const raw = localStorage.getItem("rights-app:access");
      if (raw) {
        const parsed = JSON.parse(raw);
        if (parsed.unlocked) setUnlocked(true);
      }
    } catch (e) {
      /* no saved access yet */
    }
  }, []);

  useEffect(() => {
    if (!viewedOnce.current) {
      viewedOnce.current = true;
      const t = setTimeout(() => setShowUpsell(true), 1200);
      return () => clearTimeout(t);
    }
  }, [active, scenarioIdx]);

  function selectState(code) {
    const st = STATES.find((s) => s.code === code);
    if (st.free || unlocked) {
      setActive(code);
      setScenarioIdx(0);
    } else {
      setPendingState(code);
      setGateOpen(true);
    }
  }

  async function handleUnlock(email) {
    setUnlocked(true);
    setGateOpen(false);
    try {
      localStorage.setItem(
        "rights-app:access",
        JSON.stringify({ unlocked: true, email, ts: Date.now() })
      );
    } catch (e) {
      /* storage unavailable, continue anyway */
    }

    // TODO: also send this email to your email list provider here
    // (Klaviyo, Mailchimp, etc.) via their API or a form submission,
    // tagged e.g. "rights-app-signup", so it feeds your nurture sequence.
    if (pendingState) {
      setActive(pendingState);
      setScenarioIdx(0);
      setPendingState(null);
    }
  }

  const stateData = DATA[active];
  const scenario = stateData.scenarios[scenarioIdx];
  const isLocked = !STATES.find((s) => s.code === active).free && !unlocked;

  return (
    <div
      style={{
        minHeight: "100vh",
        background: PAPER,
        fontFamily: "'PT Serif', serif",
        color: INK,
        padding: "0 0 40px",
      }}
    >
      <style>{FONTS}</style>

      {/* header */}
      <div style={{ padding: "26px 20px 10px", borderBottom: `1px solid ${INK}` }}>
        <div
          style={{
            fontFamily: "'IBM Plex Mono', monospace",
            fontSize: 11,
            letterSpacing: "0.14em",
            color: RED,
            fontWeight: 600,
            marginBottom: 6,
          }}
        >
          TRUTH TELLER JUSTICE LIBRARY
        </div>
        <h1 style={{ fontSize: 30, fontWeight: 700, margin: 0, lineHeight: 1.15 }}>
          Know Your Rights
        </h1>
        <p style={{ color: INK_FAINT, fontSize: 14, margin: "6px 0 0" }}>
          Say the right words. State by state.
        </p>
      </div>

      {/* folder tabs */}
      <div
        style={{
          display: "flex",
          gap: 3,
          padding: "16px 20px 0",
          overflowX: "auto",
        }}
      >
        {STATES.map((st) => (
          <FolderTab
            key={st.code}
            st={st}
            active={active === st.code}
            onClick={() => selectState(st.code)}
          />
        ))}
      </div>

      {/* dossier page */}
      <div
        style={{
          margin: "0 20px",
          background: "#F4EEE1",
          border: `1px solid ${INK}`,
          borderTop: "none",
          padding: "22px 20px 20px",
          position: "relative",
        }}
      >
        {/* stamp */}
        <div
          style={{
            position: "absolute",
            top: 16,
            right: 16,
            border: `2px solid ${isLocked ? INK_FAINT : RED}`,
            color: isLocked ? INK_FAINT : RED,
            fontFamily: "'IBM Plex Mono', monospace",
            fontSize: 10,
            fontWeight: 700,
            letterSpacing: "0.08em",
            padding: "4px 8px",
            transform: "rotate(6deg)",
            borderRadius: 3,
          }}
        >
          {isLocked ? "RESTRICTED" : stateData.stamp}
        </div>

        <h2 style={{ fontSize: 22, fontWeight: 700, margin: "0 0 4px" }}>
          {STATES.find((s) => s.code === active).name}
        </h2>

        {isLocked ? (
          <div style={{ padding: "40px 0 20px", textAlign: "center" }}>
            <Padlock size={28} color={INK_FAINT} />
            <p style={{ color: INK_FAINT, fontSize: 14, marginTop: 14, maxWidth: 320, marginLeft: "auto", marginRight: "auto" }}>
              This file is sealed. Enter your email to unlock all states and save your rights card for offline access.
            </p>
            <button
              onClick={() => {
                setPendingState(active);
                setGateOpen(true);
              }}
              style={{
                marginTop: 14,
                padding: "10px 20px",
                background: RED,
                color: PAPER,
                border: "none",
                fontFamily: "'IBM Plex Mono', monospace",
                fontWeight: 600,
                fontSize: 13,
                cursor: "pointer",
              }}
            >
              UNLOCK
            </button>
          </div>
        ) : (
          <>
            {/* scenario sub-tabs */}
            <div style={{ display: "flex", gap: 6, flexWrap: "wrap", margin: "12px 0 16px" }}>
              {stateData.scenarios.map((sc, i) => (
                <button
                  key={sc.title}
                  onClick={() => setScenarioIdx(i)}
                  style={{
                    fontFamily: "'IBM Plex Mono', monospace",
                    fontSize: 11,
                    letterSpacing: "0.04em",
                    padding: "6px 12px",
                    border: `1px solid ${INK}`,
                    background: scenarioIdx === i ? INK : "transparent",
                    color: scenarioIdx === i ? PAPER : INK,
                    cursor: "pointer",
                  }}
                >
                  {sc.title}
                </button>
              ))}
            </div>

            <div>
              {scenario.lines.map((ln, i) => {
                if (ln.script) return <ScriptLine key={i} text={ln.script} />;
                if (ln.label)
                  return (
                    <div
                      key={i}
                      style={{
                        fontFamily: "'IBM Plex Mono', monospace",
                        fontSize: 12,
                        fontWeight: 600,
                        color: INK_FAINT,
                        marginTop: 14,
                      }}
                    >
                      {ln.label}
                    </div>
                  );
                if (ln.note)
                  return (
                    <p
                      key={i}
                      style={{
                        fontSize: 13,
                        fontStyle: "italic",
                        color: INK_FAINT,
                        borderLeft: `2px solid ${PAPER_DARK}`,
                        paddingLeft: 10,
                        margin: "8px 0",
                        lineHeight: 1.5,
                      }}
                    >
                      {ln.note}
                    </p>
                  );
                return (
                  <p key={i} style={{ fontSize: 15, lineHeight: 1.6, margin: "10px 0" }}>
                    {ln.body}
                  </p>
                );
              })}
            </div>
          </>
        )}
      </div>

      {/* upsell */}
      {showUpsell && !isLocked && (
        <div
          style={{
            margin: "16px 20px 0",
            background: INK,
            color: PAPER,
            padding: "16px 18px",
            display: "flex",
            justifyContent: "space-between",
            alignItems: "center",
            gap: 12,
          }}
        >
          <div>
            <div style={{ fontFamily: "'IBM Plex Mono', monospace", fontSize: 11, color: "#D9A5AE", marginBottom: 4 }}>
              WANT THE FULL PICTURE?
            </div>
            <div style={{ fontSize: 13, lineHeight: 1.4, maxWidth: 260 }}>
              Volume 4 covers the courtroom side and the case law behind every right on this page.
            </div>
          </div>
          <a
            href="https://truthtellernews.com/collections/truth-teller-justice-library/products/know-your-rights-police-encounters-know-what-to-say-know-when-to-stay-silent-know-how-to-get-home-digital-ebook-instant-download"
            target="_blank"
            rel="noopener noreferrer"
            style={{
              flexShrink: 0,
              background: RED,
              color: PAPER,
              fontFamily: "'IBM Plex Mono', monospace",
              fontSize: 12,
              fontWeight: 600,
              padding: "10px 14px",
              textDecoration: "none",
              whiteSpace: "nowrap",
            }}
          >
            GET VOL. 4
          </a>
        </div>
      )}

      <p style={{ margin: "20px 20px 0", fontSize: 11, color: INK_FAINT, lineHeight: 1.5 }}>
        This is educational reference, not legal advice. Laws change — if you're facing a real charge, talk to a licensed attorney in your state.
      </p>

      {gateOpen && (
        <EmailGate
          onSubmit={handleUnlock}
          onClose={() => {
            setGateOpen(false);
            setPendingState(null);
          }}
        />
      )}
    </div>
  );
}
