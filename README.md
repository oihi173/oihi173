import React, { useState } from "react";

const panelStyles: React.CSSProperties = {
  position: "fixed",
  top: "50%",
  left: "50%",
  transform: "translate(-50%, -50%)",
  background: "#fff",
  border: "2px solid #333",
  borderRadius: "12px",
  boxShadow: "0 4px 32px rgba(0,0,0,0.18)",
  padding: 32,
  minWidth: 300,
  minHeight: 200,
  zIndex: 20
};

const coverStyles: React.CSSProperties = {
  width: 180,
  height: 180,
  borderRadius: "50%",
  border: "6px solid #333",
  background: "#ffeb3b",
  display: "flex",
  alignItems: "center",
  justifyContent: "center",
  fontSize: 60,
  cursor: "pointer",
  margin: "60px auto"
};

const overlayStyles: React.CSSProperties = {
  position: "fixed",
  top: 0,
  left: 0,
  width: "100vw",
  height: "100vh",
  background: "rgba(0,0,0,0.22)",
  zIndex: 10
};

export default function EpicFacePanel() {
  const [open, setOpen] = useState(false);

  return (
    <>
      {!open && (
        <div style={coverStyles} onClick={() => setOpen(true)} title="Clique para abrir">
          {/* Epic Face SVG */}
          <svg width="110" height="110" viewBox="0 0 110 110">
            <circle cx="55" cy="55" r="54" fill="#ffeb3b" stroke="#333" strokeWidth="4"/>
            {/* Olhos */}
            <ellipse cx="38" cy="45" rx="9" ry="13" fill="#fff" stroke="#333" strokeWidth="3"/>
            <ellipse cx="72" cy="45" rx="9" ry="13" fill="#fff" stroke="#333" strokeWidth="3"/>
            <ellipse cx="38" cy="50" rx="4" ry="6" fill="#333"/>
            <ellipse cx="72" cy="50" rx="4" ry="6" fill="#333"/>
            {/* Boca Epic */}
            <path d="M 35 65 Q 55 95 75 65" stroke="#333" strokeWidth="5" fill="none" />
          </svg>
        </div>
      )}
      {open && (
        <>
          <div style={overlayStyles} onClick={() => setOpen(false)}></div>
          <div style={panelStyles}>
            <h2 style={{ textAlign: "center", marginBottom: 16 }}>Painel Epic Face 😎</h2>
            <ul style={{ listStyle: "none", padding: 0, fontSize: 20 }}>
              <li>
                <button style={{ margin: 8, padding: "8px 20px", fontSize: 18 }}>Fica Invisível</button>
              </li>
              <li>
                <button style={{ margin: 8, padding: "8px 20px", fontSize: 18 }}>Uninvisível</button>
              </li>
              <li>
                <button style={{ margin: 8, padding: "8px 20px", fontSize: 18 }}>ESP Jogador</button>
              </li>
              <li>
                <button style={{ margin: 8, padding: "8px 20px", fontSize: 18 }}>UNESP</button>
              </li>
            </ul>
            <div style={{textAlign: "center", marginTop: 24}}>
              <button onClick={() => setOpen(false)} style={{padding: "8px 18px"}}>Fechar Painel</button>
            </div>
          </div>
        </>
      )}
    </>
  );
}
