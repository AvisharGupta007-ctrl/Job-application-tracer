import { useState } from "react";

const sections = [
  {
    id: "submission",
    title: "Application Submission",
    emoji: "📬",
    color: "#A78BFA",
    items: [
      "Double-check all form fields are filled",
      "Tailor resume to this specific job description",
      "Attach correct resume version (PDF)",
      "Write a customized cover letter",
      "Confirm contact email address is correct",
      "Note the application deadline",
      "Save confirmation email / screenshot",
      "Add to your job tracker spreadsheet",
      "Check for any required portfolio or work samples",
      "Review LinkedIn profile is up to date",
    ],
  },
  {
    id: "followup",
    title: "Follow-up & Interviews",
    emoji: "🤝",
    color: "#F59E0B",
    items: [
      "Send a follow-up email after 1 week if no response",
      "Connect with recruiter or hiring manager on LinkedIn",
      "Research interview format (panel, technical, case study…)",
      "Prepare answers to common behavioral questions (STAR method)",
      "Prepare 5 questions to ask the interviewer",
      "Plan your outfit and commute/tech setup the day before",
      "Send a thank-you email within 24h post-interview",
      "Follow up on next steps if no update after 5 business days",
      "Reflect on what went well & what to improve",
    ],
  },
];

const getProgressBarStyle = (progress) => {
  return {
    height: "100%",
    width: progress + "%",
    background: "linear-gradient(90deg, #A78BFA, #F59E0B)",
    borderRadius: 3,
    transition: "width 0.4s cubic-bezier(.4,0,.2,1)",
  };
};

const getCheckboxStyle = (isDone, color) => {
  return {
    width: 18,
    height: 18,
    borderRadius: 4,
    border: "1.5px solid " + (isDone ? color : "#2E2E3A"),
    background: isDone ? color : "transparent",
    display: "flex",
    alignItems: "center",
    justifyContent: "center",
    flexShrink: 0,
    marginTop: 1,
    transition: "all 0.2s ease",
  };
};

const getSectionHeaderStyle = (allDone, color) => {
  return {
    display: "flex",
    alignItems: "center",
    justifyContent: "space-between",
    padding: "18px 22px",
    cursor: "pointer",
    borderLeft: "3px solid " + color,
    userSelect: "none",
    background: allDone ? color + "08" : "transparent",
    transition: "background 0.3s",
  };
};

const getItemTextStyle = (isDone) => {
  return {
    fontSize: 13.5,
    color: isDone ? "#3A3A4A" : "#B8B0A0",
    textDecoration: isDone ? "line-through" : "none",
    lineHeight: 1.5,
    transition: "all 0.2s ease",
  };
};

export default function JobChecklist() {
  const [checked, setChecked] = useState({});
  const [collapsed, setCollapsed] = useState({});
  const [jobName, setJobName] = useState("");
  const [editing, setEditing] = useState(true);

  const toggle = (sectionId, idx) => {
    const key = sectionId + "-" + idx;
    setChecked((prev) => ({ ...prev, [key]: !prev[key] }));
  };

  const toggleCollapse = (id) =>
    setCollapsed((prev) => ({ ...prev, [id]: !prev[id] }));

  const totalItems = sections.reduce((a, s) => a + s.items.length, 0);
  const totalChecked = Object.values(checked).filter(Boolean).length;
  const progress = Math.round((totalChecked / totalItems) * 100);

  return (
    <div style={{
      minHeight: "100vh",
      background: "#0C0C0F",
      fontFamily: "'Georgia', serif",
      padding: "48px 20px",
      color: "#F0EBE1",
    }}>
      <div style={{ maxWidth: 600, margin: "0 auto" }}>

        {/* Header */}
        <div style={{ textAlign: "center", marginBottom: 40 }}>
          <div style={{
            fontSize: 10,
            letterSpacing: "0.3em",
            textTransform: "uppercase",
            color: "#555",
            marginBottom: 12,
          }}>
            Active Application Tracker
          </div>

          {editing ? (
            <div style={{ display: "flex", gap: 8, justifyContent: "center", alignItems: "center", flexWrap: "wrap" }}>
              <input
                value={jobName}
                onChange={(e) => setJobName(e.target.value)}
                onKeyDown={(e) => e.key === "Enter" && setEditing(false)}
                placeholder="Company name or job title..."
                style={{
                  background: "transparent",
                  border: "none",
                  borderBottom: "1px solid #333",
                  color: "#F0EBE1",
                  fontSize: 20,
                  fontFamily: "Georgia, serif",
                  textAlign: "center",
                  outline: "none",
                  padding: "6px 10px",
                  width: 270,
                }}
                autoFocus
              />
              <button
                onClick={() => setEditing(false)}
                style={{
                  background: "#A78BFA",
                  border: "none",
                  color: "#fff",
                  padding: "6px 14px",
                  borderRadius: 4,
                  cursor: "pointer",
                  fontSize: 12,
                  fontFamily: "Georgia, serif",
                  letterSpacing: "0.05em",
                }}>Set</button>
            </div>
          ) : (
            <h1
              onClick={() => setEditing(true)}
              style={{
                fontSize: 24,
                fontWeight: "normal",
                margin: 0,
                cursor: "text",
                color: "#F0EBE1",
                display: "inline-block",
                borderBottom: "1px dashed #2A2A2A",
                paddingBottom: 3,
              }}
            >
              {jobName || "My Application"}
            </h1>
          )}
        </div>

        {/* Progress */}
        <div style={{
          background: "#141418",
          border: "1px solid #222",
          borderRadius: 12,
          padding: "20px 24px",
          marginBottom: 28,
        }}>
          <div style={{
            display: "flex",
            justifyContent: "space-between",
            alignItems: "center",
            marginBottom: 12,
          }}>
            <span style={{ fontSize: 12, color: "#666", letterSpacing: "0.05em" }}>
              {totalChecked} of {totalItems} tasks done
            </span>
            <span style={{
              fontSize: 13,
              fontWeight: "bold",
              color: progress === 100 ? "#4ECDC4" : "#A78BFA",
              letterSpacing: "0.05em",
            }}>
              {progress === 100 ? "Complete!" : progress + "%"}
            </span>
          </div>
          <div style={{ height: 5, background: "#222", borderRadius: 3, overflow: "hidden" }}>
            <div style={getProgressBarStyle(progress)} />
          </div>
          {progress > 0 && progress < 100 && (
            <div style={{ marginTop: 10, fontSize: 11, color: "#444", textAlign: "right" }}>
              {totalItems - totalChecked} tasks remaining
            </div>
          )}
        </div>

        {/* Sections */}
        {sections.map((section) => {
          const sectionChecked = section.items.filter((_, i) => checked[section.id + "-" + i]).length;
          const isCollapsed = collapsed[section.id];
          const allDone = sectionChecked === section.items.length;

          return (
            <div key={section.id} style={{
              background: "#111115",
              border: "1px solid #1E1E26",
              borderRadius: 12,
              marginBottom: 16,
              overflow: "hidden",
            }}>
              <div
                onClick={() => toggleCollapse(section.id)}
                style={getSectionHeaderStyle(allDone, section.color)}
              >
                <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
                  <span style={{ fontSize: 18 }}>{section.emoji}</span>
                  <span style={{ fontSize: 15, letterSpacing: "0.02em" }}>{section.title}</span>
                  {allDone && (
                    <span style={{ fontSize: 10, color: section.color, letterSpacing: "0.1em" }}>DONE</span>
                  )}
                </div>
                <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
                  <span style={{
                    fontSize: 11,
                    color: allDone ? section.color : "#444",
                    letterSpacing: "0.05em",
                  }}>
                    {sectionChecked}/{section.items.length}
                  </span>
                  <span style={{ color: "#333", fontSize: 11 }}>{isCollapsed ? ">" : "v"}</span>
                </div>
              </div>

              {!isCollapsed && (
                <div style={{ padding: "4px 22px 18px" }}>
                  {section.items.map((item, i) => {
                    const key = section.id + "-" + i;
                    const isDone = checked[key];
                    return (
                      <div
                        key={i}
                        onClick={() => toggle(section.id, i)}
                        style={{
                          display: "flex",
                          alignItems: "flex-start",
                          gap: 12,
                          padding: "11px 0",
                          borderBottom: i < section.items.length - 1 ? "1px solid #181820" : "none",
                          cursor: "pointer",
                        }}
                      >
                        <div style={getCheckboxStyle(isDone, section.color)}>
                          {isDone && (
                            <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
                              <path d="M1 4L3.5 6.5L9 1" stroke="#fff" strokeWidth="1.8" strokeLinecap="round" strokeLinejoin="round"/>
                            </svg>
                          )}
                        </div>
                        <span style={getItemTextStyle(isDone)}>
                          {item}
                        </span>
                      </div>
                    );
                  })}
                </div>
              )}
            </div>
          );
        })}

        <div style={{ textAlign: "center", marginTop: 32, fontSize: 10, color: "#2A2A2A", letterSpacing: "0.2em" }}>
          YOU VE GOT THIS - KEEP GOING
        </div>
      </div>
    </div>
  );
}
