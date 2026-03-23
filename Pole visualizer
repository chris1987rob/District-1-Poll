import { useState, useRef, useCallback, useEffect } from "react";

const RACES = [
  {
    office: "U.S. Senator",
    instruction: "Vote for one",
    candidates: [
      { name: "Sherrod Brown", tag: null },
      { name: "Ron Kincaid", tag: null },
    ],
  },
  {
    office: "Governor / Lt. Governor",
    instruction: "Vote for one",
    candidates: [
      { name: "Amy Acton & David Pepper", tag: "Unopposed" },
    ],
  },
  {
    office: "Secretary of State",
    instruction: "Vote for one",
    candidates: [
      { name: "Bryan Hambley", tag: null },
      { name: "Allison Russo", tag: null },
    ],
  },
  {
    office: "State Representative, 1st District",
    instruction: "Vote for one",
    candidates: [
      { name: "Christopher Robbins", tag: null },
      { name: "Dontavius Jarrells", tag: "Incumbent" },
    ],
  },
  {
    office: "Attorney General",
    instruction: "Vote for one",
    candidates: [
      { name: "Eliot Forhan", tag: null },
      { name: "John J. Kulewicz", tag: null },
    ],
  },
];

const SWIPE_THRESHOLD = 90;

function BallotCard({ race, raceIndex, totalRaces, onDismiss, onVote }) {
  const cardRef = useRef(null);
  const [dragX, setDragX] = useState(0);
  const [isDragging, setIsDragging] = useState(false);
  const [isExiting, setIsExiting] = useState(false);
  const [selected, setSelected] = useState(null);
  const startX = useRef(0);

  const handleSelect = (name) => {
    if (selected) return;
    setSelected(name);
    onVote(race.office, name);
    setTimeout(() => {
      setIsExiting(true);
      setTimeout(() => onDismiss(), 380);
    }, 550);
  };

  const handlePointerDown = (e) => {
    if (e.target.closest("[data-candidate]") || selected) return;
    setIsDragging(true);
    startX.current = e.clientX;
    cardRef.current?.setPointerCapture(e.pointerId);
  };
  const handlePointerMove = (e) => {
    if (!isDragging) return;
    setDragX(Math.max(0, e.clientX - startX.current));
  };
  const handlePointerUp = (e) => {
    if (!isDragging) return;
    setIsDragging(false);
    cardRef.current?.releasePointerCapture(e.pointerId);
    if (dragX > SWIPE_THRESHOLD) {
      setIsExiting(true);
      setTimeout(() => onDismiss(), 350);
    } else {
      setDragX(0);
    }
  };

  const rot = dragX * 0.04;
  const opa = isExiting ? 0 : Math.max(0.15, 1 - dragX / 500);
  const tx = isExiting
    ? "translateX(110vw) rotate(18deg)"
    : `translateX(${dragX}px) rotate(${rot}deg)`;

  return (
    <div
      ref={cardRef}
      onPointerDown={handlePointerDown}
      onPointerMove={handlePointerMove}
      onPointerUp={handlePointerUp}
      onPointerCancel={handlePointerUp}
      style={{
        position: "absolute", top: 0, left: 0, right: 0,
        transform: tx, opacity: opa,
        transition: isDragging ? "none" : "all 0.38s cubic-bezier(.4,0,.2,1)",
        cursor: isDragging ? "grabbing" : "grab",
        touchAction: "none", userSelect: "none",
        zIndex: 50,
      }}
    >
      <div style={{
        background: "#fff",
        borderRadius: 4,
        border: "2px solid #1a1a1a",
        boxShadow: "0 8px 40px rgba(0,0,0,0.12), 0 2px 8px rgba(0,0,0,0.06)",
        overflow: "hidden",
      }}>
        {/* Blue header bar */}
        <div style={{
          background: "#003878",
          padding: "14px 24px 12px",
          display: "flex",
          justifyContent: "space-between",
          alignItems: "center",
        }}>
          <span style={{
            fontFamily: "'IBM Plex Sans', 'Helvetica Neue', Arial, sans-serif",
            fontSize: 11, fontWeight: 700,
            letterSpacing: 2.5, textTransform: "uppercase",
            color: "#fff",
          }}>
            Democratic Primary Ballot
          </span>
          <span style={{
            fontFamily: "'IBM Plex Mono', 'Courier New', monospace",
            fontSize: 11, color: "rgba(255,255,255,0.6)",
          }}>
            {raceIndex + 1} / {totalRaces}
          </span>
        </div>

        {/* Content */}
        <div style={{ padding: "24px 28px 20px" }}>
          {/* Office */}
          <div style={{
            borderBottom: "2px solid #1a1a1a",
            paddingBottom: 12,
            marginBottom: 16,
          }}>
            <h2 style={{
              fontFamily: "'IBM Plex Sans', 'Helvetica Neue', Arial, sans-serif",
              fontSize: 20, fontWeight: 700,
              color: "#111", margin: 0, lineHeight: 1.25,
            }}>
              {race.office}
            </h2>
            <p style={{
              fontFamily: "'IBM Plex Sans', Arial, sans-serif",
              fontSize: 13, color: "#555", margin: "6px 0 0",
              fontStyle: "italic",
            }}>
              {race.instruction}
            </p>
          </div>

          {/* Candidates */}
          <div style={{ display: "flex", flexDirection: "column", gap: 0 }}>
            {race.candidates.map((c, ci) => {
              const isSelected = selected === c.name;
              const isNotSelected = selected && !isSelected;

              return (
                <div
                  key={c.name}
                  data-candidate="true"
                  onClick={() => handleSelect(c.name)}
                  style={{
                    display: "flex", alignItems: "center", gap: 16,
                    padding: "14px 16px",
                    cursor: selected ? "default" : "pointer",
                    borderBottom: ci < race.candidates.length - 1 ? "1px solid #e0e0e0" : "none",
                    background: isSelected ? "#eef3fb" : "transparent",
                    opacity: isNotSelected ? 0.35 : 1,
                    transition: "all 0.3s ease",
                    borderRadius: 2,
                  }}
                  onMouseEnter={(e) => {
                    if (!selected) e.currentTarget.style.background = "#f5f7fa";
                  }}
                  onMouseLeave={(e) => {
                    if (!selected) e.currentTarget.style.background = "transparent";
                    if (isSelected) e.currentTarget.style.background = "#eef3fb";
                  }}
                >
                  {/* Ballot oval */}
                  <div style={{
                    width: 28, height: 18,
                    borderRadius: 10,
                    border: `2px solid ${isSelected ? "#003878" : "#888"}`,
                    background: isSelected ? "#003878" : "#fff",
                    flexShrink: 0,
                    display: "flex", alignItems: "center", justifyContent: "center",
                    transition: "all 0.25s ease",
                  }}>
                    {isSelected && (
                      <svg width="12" height="9" viewBox="0 0 12 9" fill="none">
                        <path d="M1 4L4.5 7.5L11 1" stroke="#fff" strokeWidth="2.2" strokeLinecap="round" strokeLinejoin="round"/>
                      </svg>
                    )}
                  </div>

                  {/* Name + tag */}
                  <div style={{ flex: 1 }}>
                    <span style={{
                      fontFamily: "'IBM Plex Sans', Arial, sans-serif",
                      fontSize: 17, fontWeight: 600,
                      color: "#111", lineHeight: 1.3,
                      display: "block",
                    }}>
                      {c.name}
                    </span>
                    {c.tag && (
                      <span style={{
                        fontFamily: "'IBM Plex Mono', 'Courier New', monospace",
                        fontSize: 10, fontWeight: 500,
                        letterSpacing: 1.2, textTransform: "uppercase",
                        color: "#666", marginTop: 2, display: "inline-block",
                      }}>
                        {c.tag}
                      </span>
                    )}
                  </div>
                </div>
              );
            })}
          </div>

          {/* Bottom instruction */}
          <div style={{
            marginTop: 20, paddingTop: 12,
            borderTop: "1px solid #e8e8e8",
            display: "flex", justifyContent: "space-between", alignItems: "center",
          }}>
            <span style={{
              fontFamily: "'IBM Plex Mono', monospace",
              fontSize: 10, color: "#999",
              letterSpacing: 0.5,
            }}>
              {selected ? "Vote recorded" : "Fill in the oval to vote"}
            </span>
            <span style={{
              fontFamily: "'IBM Plex Mono', monospace",
              fontSize: 10, color: "#bbb",
              letterSpacing: 0.5,
            }}>
              swipe to skip →
            </span>
          </div>
        </div>
      </div>
    </div>
  );
}

export default function PollVisualizer() {
  const [active, setActive] = useState(RACES.map((_, i) => i));
  const [ready, setReady] = useState(false);
  const [selections, setSelections] = useState({});

  useEffect(() => { setTimeout(() => setReady(true), 80); }, []);

  const handleDismiss = useCallback(() => {
    setActive((p) => p.slice(1));
  }, []);

  const handleVote = useCallback((office, name) => {
    setSelections((p) => ({ ...p, [office]: name }));
  }, []);

  const handleReset = () => {
    setActive(RACES.map((_, i) => i));
    setSelections({});
  };

  const done = active.length === 0;
  const selCount = Object.keys(selections).length;

  return (
    <div style={{
      minHeight: "100vh",
      background: "#f0f0ec",
      display: "flex", flexDirection: "column",
      alignItems: "center", justifyContent: "flex-start",
      padding: "40px 16px",
      fontFamily: "'IBM Plex Sans', 'Helvetica Neue', Arial, sans-serif",
    }}>
      <link
        href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;700&display=swap"
        rel="stylesheet"
      />

      {/* Card area - now first */}
      <div style={{
        position: "relative",
        width: "100%", maxWidth: 460,
        minHeight: done ? "auto" : 320,
        opacity: ready ? 1 : 0,
        transform: ready ? "scale(1)" : "scale(0.97)",
        transition: "all 0.5s ease 0.1s",
      }}>
        {done ? (
          <div style={{
            background: "#fff", border: "2px solid #1a1a1a",
            borderRadius: 4, overflow: "hidden",
          }}>
            <div style={{
              background: "#003878", padding: "14px 24px",
              display: "flex", justifyContent: "space-between", alignItems: "center",
            }}>
              <span style={{
                fontFamily: "'IBM Plex Sans', Arial, sans-serif",
                fontSize: 11, fontWeight: 700,
                letterSpacing: 2.5, textTransform: "uppercase", color: "#fff",
              }}>
                Ballot Summary
              </span>
              <span style={{
                fontFamily: "'IBM Plex Mono', monospace",
                fontSize: 11, color: "rgba(255,255,255,0.6)",
              }}>
                {selCount} / {RACES.length}
              </span>
            </div>
            <div style={{ padding: "20px 24px" }}>
              {RACES.map((race, i) => {
                const pick = selections[race.office];
                return (
                  <div key={race.office} style={{
                    display: "flex", justifyContent: "space-between", alignItems: "center",
                    padding: "12px 0",
                    borderBottom: i < RACES.length - 1 ? "1px solid #e8e8e8" : "none",
                  }}>
                    <span style={{
                      fontFamily: "'IBM Plex Sans', Arial, sans-serif",
                      fontSize: 13, color: "#666", fontWeight: 500,
                    }}>
                      {race.office}
                    </span>
                    <div style={{ display: "flex", alignItems: "center", gap: 8 }}>
                      {pick && (
                        <div style={{
                          width: 16, height: 10, borderRadius: 6,
                          background: "#003878",
                          display: "flex", alignItems: "center", justifyContent: "center",
                        }}>
                          <svg width="7" height="5" viewBox="0 0 12 9" fill="none">
                            <path d="M1 4L4.5 7.5L11 1" stroke="#fff" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"/>
                          </svg>
                        </div>
                      )}
                      <span style={{
                        fontFamily: "'IBM Plex Sans', Arial, sans-serif",
                        fontSize: 13, fontWeight: 600,
                        color: pick ? "#111" : "#bbb",
                      }}>
                        {pick || "Skipped"}
                      </span>
                    </div>
                  </div>
                );
              })}

              <button
                onClick={() => {
                  alert("Ballot submitted! Thank you for voting.");
                }}
                style={{
                  marginTop: 20, width: "100%",
                  fontFamily: "'IBM Plex Sans', Arial, sans-serif",
                  fontSize: 13, fontWeight: 600,
                  letterSpacing: 1, textTransform: "uppercase",
                  background: "#003878", color: "#fff",
                  border: "none", borderRadius: 2,
                  padding: "12px 24px", cursor: "pointer",
                  transition: "background 0.2s",
                }}
                onMouseEnter={(e) => e.currentTarget.style.background = "#004a9e"}
                onMouseLeave={(e) => e.currentTarget.style.background = "#003878"}
              >
                Submit
              </button>
              <button
                onClick={handleReset}
                style={{
                  marginTop: 8, width: "100%",
                  fontFamily: "'IBM Plex Sans', Arial, sans-serif",
                  fontSize: 13, fontWeight: 600,
                  letterSpacing: 1, textTransform: "uppercase",
                  background: "#fff", color: "#003878",
                  border: "2px solid #003878", borderRadius: 2,
                  padding: "10px 24px", cursor: "pointer",
                  transition: "all 0.2s",
                }}
                onMouseEnter={(e) => {
                  e.currentTarget.style.background = "#f0f4f9";
                }}
                onMouseLeave={(e) => {
                  e.currentTarget.style.background = "#fff";
                }}
              >
                Retake Poll
              </button>
            </div>
          </div>
        ) : (
          <>
            {active.length > 1 && (
              <div style={{
                position: "absolute", top: 6, left: 4, right: -4, bottom: -6,
                background: "#fff", border: "2px solid #ccc",
                borderRadius: 4, zIndex: 1,
              }} />
            )}
            {active.length > 2 && (
              <div style={{
                position: "absolute", top: 12, left: 8, right: -8, bottom: -12,
                background: "#f8f8f6", border: "2px solid #ddd",
                borderRadius: 4, zIndex: 0,
              }} />
            )}

            <BallotCard
              key={active[0]}
              race={RACES[active[0]]}
              raceIndex={RACES.length - active.length}
              totalRaces={RACES.length}
              onDismiss={handleDismiss}
              onVote={handleVote}
            />
          </>
        )}
      </div>

      {/* Progress bar */}
      {!done && (
        <div style={{
          marginTop: 28, width: "100%", maxWidth: 460,
          opacity: ready ? 1 : 0,
          transition: "opacity 0.4s 0.2s",
        }}>
          <div style={{
            height: 3, background: "#ddd", borderRadius: 2,
            overflow: "hidden",
          }}>
            <div style={{
              height: "100%",
              width: `${((RACES.length - active.length) / RACES.length) * 100}%`,
              background: "#003878",
              borderRadius: 2,
              transition: "width 0.5s cubic-bezier(.4,0,.2,1)",
            }} />
          </div>
        </div>
      )}

      {/* Branding + race info below card */}
      <div style={{
        textAlign: "center", marginTop: 28,
        opacity: ready ? 1 : 0,
        transform: ready ? "translateY(0)" : "translateY(12px)",
        transition: "all 0.5s ease 0.2s",
      }}>
        <p style={{
          fontFamily: "'IBM Plex Sans', Arial, sans-serif",
          fontSize: 14, color: "#666", margin: "0 0 10px",
        }}>
          {done
            ? `${selCount} of ${RACES.length} races voted`
            : `Race ${RACES.length - active.length + 1} of ${RACES.length}`
          }
        </p>
        <div style={{
          display: "inline-block",
          background: "#003878", color: "#fff",
          fontFamily: "'IBM Plex Mono', monospace",
          fontSize: 10, fontWeight: 700,
          letterSpacing: 3, textTransform: "uppercase",
          padding: "5px 16px", borderRadius: 2,
        }}>
          Robbins for Ohio
        </div>
      </div>
    </div>
  );
}