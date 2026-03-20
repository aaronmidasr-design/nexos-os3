# nexos-os3
my ai
[index (3).html](https://github.com/user-attachments/files/26131577/index.3.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>NEXOS OS</title>
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.5/babel.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
html,body,#root{width:100%;height:100%;overflow:hidden;background:#000008}
::-webkit-scrollbar{width:4px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:#2A2A40;border-radius:4px}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:0.4}}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-6px)}}
@keyframes fadeIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}
@keyframes slideIn{from{opacity:0;transform:scale(0.96)}to{opacity:1;transform:scale(1)}}
input,textarea,button{font-family:inherit}
</style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState, useEffect, useRef, useCallback } = React;

const API_KEY_STORAGE = "nexos_api_key";

const APPS = {
  chat:     { id:"chat",     label:"IA",      icon:"◈", color:"#7C3AED" },
  notes:    { id:"notes",    label:"Notas",   icon:"◇", color:"#0EA5E9" },
  terminal: { id:"terminal", label:"Terminal",icon:"▸", color:"#10B981" },
  files:    { id:"files",    label:"Arquivos",icon:"⬡", color:"#F59E0B" },
  about:    { id:"about",    label:"Sobre",   icon:"♦", color:"#EC4899" },
};

const INITIAL_NOTES = [
  { id:1, title:"Boas-vindas", content:"Bem-vindo ao NEXOS OS!\n\nEste e o seu espaco pessoal.\nEdite, crie, explore a vontade.", pinned:true },
  { id:2, title:"Como usar",   content:"IA - converse com inteligencia artificial\nNotas - escreva suas ideias\nTerminal - rode comandos\nArquivos - gerencie arquivos", pinned:true },
  { id:3, title:"Ideias",      content:"- Adicionar mais apps\n- Personalizar cores\n- Exportar notas", pinned:false },
];

const FILES = [
  { name:"projetos/",       type:"dir", size:"--",     date:"hoje"   },
  { name:"notas.md",        type:"md",  size:"2 KB",   date:"ontem"  },
  { name:"ideias.txt",      type:"txt", size:"512 B",  date:"3 dias" },
  { name:"foto.png",        type:"img", size:"84 KB",  date:"semana" },
  { name:"curriculo.pdf",   type:"pdf", size:"122 KB", date:"mes"    },
  { name:"config.json",     type:"json",size:"1 KB",   date:"hoje"   },
];

function ApiKeyModal({ onSave, onSkip }) {
  const [key, setKey] = useState("");
  const [show, setShow] = useState(false);
  const [err, setErr] = useState("");

  const save = () => {
    const k = key.trim();
    if (!k) { setErr("Cole sua API Key aqui."); return; }
    if (!k.startsWith("sk-ant-")) { setErr("Chave invalida. Deve comecar com sk-ant-"); return; }
    localStorage.setItem(API_KEY_STORAGE, k);
    onSave(k);
  };

  return (
    <div style={{ position:"fixed", inset:0, background:"rgba(0,0,8,0.97)", display:"flex", alignItems:"center", justifyContent:"center", zIndex:99999, fontFamily:"'Syne',sans-serif" }}>
      <div style={{ background:"#0D0D18", border:"1px solid #2A2A40", borderRadius:18, padding:36, maxWidth:440, width:"90%", boxShadow:"0 32px 80px rgba(0,0,0,0.8)" }}>
        <div style={{ fontSize:40, textAlign:"center", marginBottom:6 }}>◈</div>
        <h2 style={{ color:"#E8E8F0", fontSize:22, fontWeight:800, textAlign:"center", marginBottom:4 }}>NEXOS OS</h2>
        <p style={{ color:"#555570", fontSize:12, textAlign:"center", marginBottom:28, lineHeight:1.6 }}>
          Ative a IA com sua chave da Anthropic ou use sem IA gratuitamente
        </p>
        <label style={{ color:"#8888A8", fontSize:11, fontWeight:700, letterSpacing:1 }}>API KEY (opcional)</label>
        <div style={{ position:"relative", marginTop:8, marginBottom:6 }}>
          <input
            type={show ? "text" : "password"}
            value={key}
            onChange={e => { setKey(e.target.value); setErr(""); }}
            onKeyDown={e => e.key === "Enter" && save()}
            placeholder="sk-ant-..."
            style={{ width:"100%", background:"#1A1A2E", border:"1px solid #2A2A40", borderRadius:10, padding:"10px 40px 10px 14px", color:"#E8E8F0", fontSize:13, outline:"none" }}
          />
          <button onClick={() => setShow(s => !s)} style={{ position:"absolute", right:10, top:"50%", transform:"translateY(-50%)", background:"none", border:"none", color:"#555570", cursor:"pointer", fontSize:14 }}>
            {show ? "X" : "O"}
          </button>
        </div>
        {err && <p style={{ color:"#EF4444", fontSize:11, marginBottom:8 }}>{err}</p>}
        <button onClick={save} style={{ width:"100%", background:"#7C3AED", border:"none", borderRadius:10, padding:11, color:"#fff", fontSize:13, fontWeight:700, cursor:"pointer", marginBottom:10 }}>
          Entrar com IA
        </button>
        <button onClick={onSkip} style={{ width:"100%", background:"none", border:"1px solid #2A2A40", borderRadius:10, padding:10, color:"#8888A8", fontSize:13, cursor:"pointer", marginBottom:20 }}>
          Usar sem IA (gratis)
        </button>
        <p style={{ color:"#333350", fontSize:11, textAlign:"center", lineHeight:1.7 }}>
          Sua chave fica salva so no seu navegador.
          Pegue sua chave em console.anthropic.com
        </p>
      </div>
    </div>
  );
}

function ChatApp({ apiKey, onNeedKey }) {
  const [messages, setMessages] = useState([{
    role:"assistant",
    text: apiKey
      ? "Ola! Sou sua IA integrada ao NEXOS. Tenho acesso a internet. Pergunte qualquer coisa!"
      : "Ola! Estou no modo offline. Clique em Ativar IA para me ativar.",
  }]);
  const [input, setInput] = useState("");
  const [loading, setLoading] = useState(false);
  const [status, setStatus] = useState("");
  const [webOn, setWebOn] = useState(true);
  const bottomRef = useRef(null);

  useEffect(() => { bottomRef.current && bottomRef.current.scrollIntoView({ behavior:"smooth" }); }, [messages, status]);

  const send = useCallback(async () => {
    if (!apiKey) { onNeedKey(); return; }
    const text = input.trim();
    if (!text || loading) return;
    setInput("");
    const newMsgs = [...messages, { role:"user", text }];
    setMessages(newMsgs);
    setLoading(true);
    setStatus(webOn ? "Buscando na web..." : "Pensando...");

    try {
      const history = newMsgs.map(m => ({ role: m.role === "assistant" ? "assistant" : "user", content: m.text }));
      const body = {
        model: "claude-sonnet-4-20250514",
        max_tokens: 1000,
        system: "Voce e a IA integrada ao NEXOS OS. Seja conciso e util. Responda sempre em portugues.",
        messages: history,
      };
      const headers = {
        "Content-Type": "application/json",
        "x-api-key": apiKey,
        "anthropic-version": "2023-06-01",
        "anthropic-dangerous-direct-browser-access": "true",
      };
      if (webOn) {
        headers["anthropic-beta"] = "web-search-2025-03-05";
        body.tools = [{ type:"web_search_20250305", name:"web_search" }];
      }

      const res = await fetch("https://api.anthropic.com/v1/messages", { method:"POST", headers, body:JSON.stringify(body) });
      const data = await res.json();
      if (data.error) throw new Error(data.error.message);

      if (data.stop_reason === "tool_use") {
        setStatus("Analisando resultados...");
        const toolUse = data.content.find(b => b.type === "tool_use");
        const body2 = {
          model: "claude-sonnet-4-20250514", max_tokens: 1000,
          system: body.system, tools: body.tools,
          messages: [
            ...history,
            { role:"assistant", content: data.content },
            { role:"user", content:[{ type:"tool_result", tool_use_id: toolUse.id, content:"Responda em portugues." }] },
          ],
        };
        const res2 = await fetch("https://api.anthropic.com/v1/messages", { method:"POST", headers, body:JSON.stringify(body2) });
        const data2 = await res2.json();
        const reply = data2.content && data2.content.find(b => b.type === "text") ? data2.content.find(b => b.type === "text").text : "Sem resposta.";
        setMessages(m => [...m, { role:"assistant", text:reply, searched:true }]);
      } else {
        const blocks = data.content ? data.content.filter(b => b.type === "text") : [];
        const reply = blocks.length > 0 ? blocks.map(b => b.text).join("\n") : "Sem resposta.";
        setMessages(m => [...m, { role:"assistant", text:reply, searched:false }]);
      }
    } catch(e) {
      setMessages(m => [...m, { role:"assistant", text:"Erro: " + e.message }]);
    } finally {
      setLoading(false);
      setStatus("");
    }
  }, [input, loading, messages, webOn, apiKey]);

  return (
    <div style={{ display:"flex", flexDirection:"column", height:"100%", background:"#0D0D14" }}>
      <div style={{ padding:"8px 14px", borderBottom:"1px solid #1A1A2E", display:"flex", alignItems:"center", gap:8 }}>
        {apiKey ? (
          <>
            <button onClick={() => setWebOn(v => !v)} style={{ padding:"4px 12px", borderRadius:20, fontSize:11, fontWeight:700, cursor:"pointer", border: webOn ? "1px solid #10B98166" : "1px solid #2A2A40", background: webOn ? "#10B98118" : "transparent", color: webOn ? "#10B981" : "#555570" }}>
              {webOn ? "Web ON" : "Web OFF"}
            </button>
            <span style={{ color:"#444460", fontSize:11 }}>{webOn ? "Busca ativa" : "Modo offline"}</span>
          </>
        ) : (
          <button onClick={onNeedKey} style={{ padding:"4px 14px", borderRadius:20, fontSize:11, fontWeight:700, cursor:"pointer", border:"1px solid #7C3AED66", background:"#7C3AED18", color:"#7C3AED" }}>
            + Ativar IA
          </button>
        )}
      </div>
      <div style={{ flex:1, overflowY:"auto", padding:20, display:"flex", flexDirection:"column", gap:12 }}>
        {messages.map((m, i) => (
          <div key={i} style={{ display:"flex", flexDirection:"column", alignItems: m.role === "user" ? "flex-end" : "flex-start" }}>
            {m.searched && <span style={{ fontSize:10, color:"#10B981", marginBottom:4 }}>buscado na web</span>}
            <div style={{ maxWidth:"75%", padding:"10px 14px", whiteSpace:"pre-wrap", lineHeight:1.65, fontSize:13, color:"#E8E8F0", borderRadius: m.role === "user" ? "18px 18px 4px 18px" : "18px 18px 18px 4px", background: m.role === "user" ? "#7C3AED" : "#1A1A2E", border: m.role === "assistant" ? "1px solid #2A2A40" : "none" }}>
              {m.text}
            </div>
          </div>
        ))}
        {loading && (
          <div style={{ display:"flex", flexDirection:"column", gap:6 }}>
            {status && <span style={{ fontSize:11, color:"#10B981", animation:"pulse 1.2s infinite" }}>{status}</span>}
            <div style={{ padding:"10px 14px", borderRadius:"18px 18px 18px 4px", background:"#1A1A2E", border:"1px solid #2A2A40", color:"#7C3AED", fontSize:13, width:"fit-content", animation:"pulse 1s infinite" }}>pensando...</div>
          </div>
        )}
        <div ref={bottomRef}/>
      </div>
      <div style={{ padding:"12px 16px", borderTop:"1px solid #1A1A2E", display:"flex", gap:8 }}>
        <input value={input} onChange={e => setInput(e.target.value)} onKeyDown={e => e.key === "Enter" && send()}
          placeholder={apiKey ? "Pergunte algo..." : "Ative a IA para conversar..."}
          style={{ flex:1, background:"#1A1A2E", border:"1px solid #2A2A40", borderRadius:10, padding:"9px 14px", color:"#E8E8F0", fontSize:13, outline:"none" }}/>
        <button onClick={send} disabled={loading} style={{ background: loading ? "#3A2A6E" : "#7C3AED", border:"none", borderRadius:10, padding:"9px 18px", color:"#fff", fontSize:14, cursor: loading ? "not-allowed" : "pointer", fontWeight:700 }}>
          &uarr;
        </button>
      </div>
    </div>
  );
}

function NotesApp() {
  const [notes, setNotes] = useState(INITIAL_NOTES);
  const [selected, setSelected] = useState(1);

  const current = notes.find(n => n.id === selected);
  const updateNote = (f, v) => setNotes(ns => ns.map(n => n.id === selected ? {...n, [f]:v} : n));
  const addNote = () => { const id = Date.now(); setNotes(ns => [...ns, { id, title:"Nova nota", content:"", pinned:false }]); setSelected(id); };
  const deleteNote = id => { const r = notes.filter(n => n.id !== id); setNotes(r); if (selected === id) setSelected(r[0] && r[0].id); };
  const togglePin = id => setNotes(ns => ns.map(n => n.id === id ? {...n, pinned:!n.pinned} : n));

  const sorted = [...notes.filter(n => n.pinned), ...notes.filter(n => !n.pinned)];

  return (
    <div style={{ display:"flex", height:"100%", background:"#0A0A12" }}>
      <div style={{ width:210, borderRight:"1px solid #1A1A2E", display:"flex", flexDirection:"column", flexShrink:0 }}>
        <div style={{ padding:"10px 14px", borderBottom:"1px solid #1A1A2E", display:"flex", justifyContent:"space-between", alignItems:"center" }}>
          <span style={{ color:"#8888A8", fontSize:11, fontWeight:700, textTransform:"uppercase", letterSpacing:1 }}>Notas ({notes.length})</span>
          <button onClick={addNote} style={{ background:"none", border:"none", color:"#0EA5E9", fontSize:20, cursor:"pointer", lineHeight:1, padding:0 }}>+</button>
        </div>
        <div style={{ flex:1, overflowY:"auto" }}>
          {sorted.map(n => (
            <div key={n.id} onClick={() => setSelected(n.id)} style={{ padding:"9px 14px", cursor:"pointer", borderBottom:"1px solid #0D0D1A", background: selected === n.id ? "#151525" : "transparent", display:"flex", justifyContent:"space-between", alignItems:"flex-start", gap:4 }}>
              <div style={{ flex:1, minWidth:0 }}>
                <div style={{ color: selected === n.id ? "#0EA5E9" : "#C8C8E0", fontSize:12.5, fontWeight:600, overflow:"hidden", textOverflow:"ellipsis", whiteSpace:"nowrap" }}>
                  {n.pinned ? "* " : ""}{n.title}
                </div>
                <div style={{ color:"#555570", fontSize:11, marginTop:2, overflow:"hidden", textOverflow:"ellipsis", whiteSpace:"nowrap" }}>{n.content.slice(0,30)}{n.content.length > 30 ? "..." : ""}</div>
              </div>
              <div style={{ display:"flex", gap:3, flexShrink:0 }}>
                <button onClick={e => { e.stopPropagation(); togglePin(n.id); }} style={{ background:"none", border:"none", color: n.pinned ? "#0EA5E9" : "#333350", cursor:"pointer", fontSize:10, padding:2 }}>*</button>
                <button onClick={e => { e.stopPropagation(); deleteNote(n.id); }} style={{ background:"none", border:"none", color:"#333350", cursor:"pointer", fontSize:16, padding:2, lineHeight:1 }}>x</button>
              </div>
            </div>
          ))}
        </div>
        <div style={{ padding:"8px 14px", borderTop:"1px solid #1A1A2E", color:"#333350", fontSize:10 }}>{notes.length} notas</div>
      </div>
      {current ? (
        <div style={{ flex:1, display:"flex", flexDirection:"column" }}>
          <div style={{ padding:"12px 20px", borderBottom:"1px solid #1A1A2E" }}>
            <input value={current.title} onChange={e => updateNote("title", e.target.value)} style={{ background:"none", border:"none", outline:"none", color:"#E8E8F0", fontSize:18, fontWeight:700, fontFamily:"inherit", width:"100%" }}/>
          </div>
          <textarea value={current.content} onChange={e => updateNote("content", e.target.value)} placeholder="Escreva aqui..." style={{ flex:1, background:"none", border:"none", outline:"none", resize:"none", color:"#B8B8D0", fontSize:13.5, lineHeight:1.85, fontFamily:"inherit", padding:"16px 20px" }}/>
          <div style={{ padding:"8px 20px", borderTop:"1px solid #1A1A2E", color:"#333350", fontSize:10 }}>{current.content.length} chars</div>
        </div>
      ) : (
        <div style={{ flex:1, display:"flex", alignItems:"center", justifyContent:"center", color:"#333350", flexDirection:"column", gap:8 }}>
          <span style={{fontSize:32}}>◇</span>
          <span style={{fontSize:13}}>Selecione ou crie uma nota</span>
        </div>
      )}
    </div>
  );
}

function TerminalApp() {
  const [history, setHistory] = useState([
    {type:"sys", text:"NEXOS v1.0 - Sistema Pessoal"},
    {type:"sys", text:"----------------------------"},
    {type:"sys", text:'Digite "help" para ver os comandos.'},
  ]);
  const [input, setInput] = useState("");
  const [cmdHist, setCmdHist] = useState([]);
  const [histIdx, setHistIdx] = useState(-1);
  const bottomRef = useRef(null);

  useEffect(() => { bottomRef.current && bottomRef.current.scrollIntoView(); }, [history]);

  const run = () => {
    const cmd = input.trim();
    if (!cmd) return;
    setCmdHist(h => [cmd, ...h]);
    setHistIdx(-1);
    setInput("");
    const out = [{type:"cmd", text:"nexos@os:~$ " + cmd}];
    const c = cmd.toLowerCase();

    if (c === "help") {
      out.push({type:"sys", text:"Comandos:"});
      var cmds = [["help","mostra este menu"],["ls","lista arquivos"],["clear","limpa terminal"],
        ["whoami","exibe usuario"],["date","data e hora"],["echo x","repete texto"],
        ["calc x","calculadora"],["neofetch","info do sistema"],["joke","piada"]];
      cmds.forEach(function(item) { out.push({type:"info", text:"  " + item[0] + " - " + item[1]}); });
    } else if (c === "ls") {
      FILES.forEach(function(f) { out.push({type:"info", text:"  " + (f.type === "dir" ? "[DIR]" : "[ARQ]") + " " + f.name + "  " + f.size}); });
    } else if (c === "clear") {
      setHistory([]);
      return;
    } else if (c === "whoami") {
      out.push({type:"info", text:"nexos_user"});
    } else if (c === "date") {
      out.push({type:"info", text: new Date().toLocaleString("pt-BR")});
    } else if (c.indexOf("echo ") === 0) {
      out.push({type:"info", text: cmd.slice(5)});
    } else if (c.indexOf("calc ") === 0) {
      try {
        var result = Function('"use strict"; return (' + cmd.slice(5) + ')')();
        out.push({type:"info", text:"= " + result});
      } catch(e) {
        out.push({type:"err", text:"Expressao invalida"});
      }
    } else if (c === "joke") {
      var jokes = ["Por que o programador foi ao medico? Tinha um bug na cabeca.","O que o zero disse pro oito? Bonito cinto!","Por que programadores preferem o escuro? Light atrai bugs!","404: Piada nao encontrada."];
      out.push({type:"info", text: jokes[Math.floor(Math.random() * jokes.length)]});
    } else if (c === "neofetch") {
      out.push({type:"info", text:"  OS         NEXOS Personal v1.0"});
      out.push({type:"info", text:"  Kernel     React 18"});
      out.push({type:"info", text:"  Shell      nexosh v1.0"});
      out.push({type:"info", text:"  Resolucao  " + window.innerWidth + "x" + window.innerHeight});
      out.push({type:"info", text:"  Memoria    infinita (cloud)"});
      out.push({type:"info", text:"  Tema       Dark Nebula"});
    } else {
      out.push({type:"err", text:"nexosh: comando nao encontrado: " + cmd});
    }

    setHistory(h => [...h, ...out]);
  };

  const onKey = e => {
    if (e.key === "Enter") run();
    if (e.key === "ArrowUp") { var i = Math.min(histIdx + 1, cmdHist.length - 1); setHistIdx(i); setInput(cmdHist[i] || ""); }
    if (e.key === "ArrowDown") { var j = Math.max(histIdx - 1, -1); setHistIdx(j); setInput(j === -1 ? "" : cmdHist[j]); }
  };

  var colors = {sys:"#10B981", cmd:"#6668A0", info:"#C8C8E0", err:"#EF4444"};

  return (
    <div style={{ height:"100%", background:"#050A05", fontFamily:"'JetBrains Mono',monospace", display:"flex", flexDirection:"column", padding:16, boxSizing:"border-box" }}>
      <div style={{ flex:1, overflowY:"auto", display:"flex", flexDirection:"column", gap:1 }}>
        {history.map((h, i) => <div key={i} style={{color: colors[h.type] || "#C8C8E0", fontSize:12.5, lineHeight:1.7}}>{h.text}</div>)}
        <div ref={bottomRef}/>
      </div>
      <div style={{ display:"flex", alignItems:"center", gap:8, marginTop:8, borderTop:"1px solid #0D1F0D", paddingTop:10 }}>
        <span style={{color:"#10B981", fontSize:12.5, flexShrink:0}}>nexos@os:~$</span>
        <input autoFocus value={input} onChange={e => setInput(e.target.value)} onKeyDown={onKey}
          style={{flex:1, background:"none", border:"none", outline:"none", color:"#10B981", fontSize:12.5, fontFamily:"inherit", caretColor:"#10B981"}}/>
      </div>
    </div>
  );
}

function FilesApp() {
  const [selected, setSelected] = useState(null);
  const [view, setView] = useState("grid");
  var icons = {dir:"📁", md:"📝", txt:"📄", img:"🖼", pdf:"📕", json:"⚙"};

  return (
    <div style={{height:"100%", background:"#0A0A12", display:"flex", flexDirection:"column"}}>
      <div style={{padding:"10px 16px", borderBottom:"1px solid #1A1A2E", display:"flex", gap:8, alignItems:"center", justifyContent:"space-between"}}>
        <span style={{color:"#8888A8", fontSize:11}}>nexos / home / user</span>
        <div style={{display:"flex", gap:6}}>
          {["grid","list"].map(v => (
            <button key={v} onClick={() => setView(v)} style={{background: view === v ? "#1A1A2E" : "none", border: "1px solid " + (view === v ? "#2A2A40" : "transparent"), borderRadius:6, padding:"3px 8px", color: view === v ? "#E8E8F0" : "#555570", fontSize:11, cursor:"pointer"}}>
              {v === "grid" ? "Grid" : "Lista"}
            </button>
          ))}
        </div>
      </div>
      {view === "grid" ? (
        <div style={{flex:1, padding:16, display:"grid", gridTemplateColumns:"repeat(auto-fill,minmax(110px,1fr))", gap:8, alignContent:"start", overflowY:"auto"}}>
          {FILES.map(f => (
            <div key={f.name} onClick={() => setSelected(selected === f.name ? null : f.name)}
              style={{padding:"14px 8px", borderRadius:10, border:"1px solid " + (selected === f.name ? "#F59E0B44" : "#1A1A2E"), background: selected === f.name ? "#1A1508" : "#0F0F1C", cursor:"pointer", textAlign:"center", transition:"all 0.15s"}}>
              <div style={{fontSize:28}}>{icons[f.type] || "📄"}</div>
              <div style={{color: selected === f.name ? "#F59E0B" : "#B8B8D0", fontSize:11, marginTop:6, wordBreak:"break-all"}}>{f.name}</div>
              <div style={{color:"#444460", fontSize:10, marginTop:2}}>{f.size}</div>
            </div>
          ))}
        </div>
      ) : (
        <div style={{flex:1, overflowY:"auto"}}>
          {FILES.map(f => (
            <div key={f.name} onClick={() => setSelected(selected === f.name ? null : f.name)}
              style={{display:"grid", gridTemplateColumns:"1fr auto auto", padding:"8px 16px", borderBottom:"1px solid #0D0D1A", background: selected === f.name ? "#1A1508" : "transparent", cursor:"pointer", alignItems:"center"}}>
              <span style={{color: selected === f.name ? "#F59E0B" : "#C8C8E0", fontSize:12.5, display:"flex", alignItems:"center", gap:8}}>{icons[f.type] || "📄"} {f.name}</span>
              <span style={{color:"#555570", fontSize:11}}>{f.size}</span>
              <span style={{color:"#444460", fontSize:11, paddingLeft:24}}>{f.date}</span>
            </div>
          ))}
        </div>
      )}
      {selected && (
        <div style={{borderTop:"1px solid #1A1A2E", padding:"10px 16px", display:"flex", justifyContent:"space-between", alignItems:"center", background:"#0D0D18"}}>
          <span style={{color:"#8888A8", fontSize:12}}>Selecionado: <strong style={{color:"#F59E0B"}}>{selected}</strong></span>
          <button onClick={() => setSelected(null)} style={{background:"none", border:"none", color:"#555570", cursor:"pointer", fontSize:16}}>x</button>
        </div>
      )}
    </div>
  );
}

function AboutApp() {
  return (
    <div style={{height:"100%", background:"#0A0A12", display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", gap:16, padding:32, textAlign:"center"}}>
      <div style={{fontSize:56, animation:"float 4s ease-in-out infinite"}}>◈</div>
      <div>
        <div style={{color:"#E8E8F0", fontSize:24, fontWeight:800, letterSpacing:-1}}>NEXOS OS</div>
        <div style={{color:"#7C3AED", fontSize:12, fontWeight:700, letterSpacing:3, marginTop:4}}>VERSAO 1.0</div>
      </div>
      <div style={{maxWidth:300, color:"#555570", fontSize:13, lineHeight:1.8}}>
        Um sistema operacional pessoal rodando no navegador.
      </div>
      <div style={{display:"grid", gridTemplateColumns:"1fr 1fr", gap:10, width:"100%", maxWidth:300}}>
        {[["IA","Chat com inteligencia artificial"],["Notas","Escreva suas ideias"],["Terminal","Rode comandos"],["Arquivos","Gerencie arquivos"]].map(function(item) {
          return (
            <div key={item[0]} style={{background:"#111120", border:"1px solid #1A1A2E", borderRadius:10, padding:"12px 14px", textAlign:"left"}}>
              <div style={{color:"#E8E8F0", fontSize:12, fontWeight:700}}>{item[0]}</div>
              <div style={{color:"#444460", fontSize:11, marginTop:4}}>{item[1]}</div>
            </div>
          );
        })}
      </div>
    </div>
  );
}

function Window({ app, apiKey, onNeedKey, onClose, onFocus, zIndex, initialPos }) {
  const [pos, setPos] = useState(initialPos);
  const [max, setMax] = useState(false);

  const onMouseDown = e => {
    if (max) return;
    onFocus();
    var sx = e.clientX - pos.x, sy = e.clientY - pos.y;
    var move = function(me) { setPos({x: me.clientX - sx, y: me.clientY - sy}); };
    var up = function() { window.removeEventListener("mousemove", move); window.removeEventListener("mouseup", up); };
    window.addEventListener("mousemove", move);
    window.addEventListener("mouseup", up);
  };

  var AppMap = {chat:ChatApp, notes:NotesApp, terminal:TerminalApp, files:FilesApp, about:AboutApp};
  var AppComponent = AppMap[app.id];
  var style = max
    ? {position:"fixed", top:32, left:0, right:0, bottom:0, zIndex:zIndex, borderRadius:0}
    : {position:"fixed", top:pos.y, left:pos.x, width:540, height:420, zIndex:zIndex, borderRadius:12};

  return (
    <div onClick={onFocus} style={{...style, background:"#0D0D18", border:"1px solid #2A2A40", boxShadow:"0 24px 80px rgba(0,0,0,0.7)", display:"flex", flexDirection:"column", overflow:"hidden", animation:"slideIn 0.2s ease"}}>
      <div onMouseDown={onMouseDown} style={{height:36, display:"flex", alignItems:"center", padding:"0 12px", background:"#111120", borderBottom:"1px solid #1A1A2E", cursor: max ? "default" : "move", userSelect:"none", gap:8, flexShrink:0}}>
        <div style={{display:"flex", gap:6}}>
          <div onClick={e => { e.stopPropagation(); onClose(); }} style={{width:12, height:12, borderRadius:"50%", background:"#EF4444", cursor:"pointer"}}/>
          <div style={{width:12, height:12, borderRadius:"50%", background:"#F59E0B"}}/>
          <div onClick={e => { e.stopPropagation(); setMax(m => !m); }} style={{width:12, height:12, borderRadius:"50%", background:"#10B981", cursor:"pointer"}}/>
        </div>
        <div style={{flex:1, textAlign:"center", color:"#6668A0", fontSize:11, fontWeight:600, letterSpacing:1}}>{app.icon} {app.label.toUpperCase()}</div>
      </div>
      <div style={{flex:1, overflow:"hidden"}}>
        <AppComponent apiKey={apiKey} onNeedKey={onNeedKey}/>
      </div>
    </div>
  );
}

function NexosOS() {
  const [apiKey, setApiKey] = useState(localStorage.getItem(API_KEY_STORAGE) || "");
  const [showModal, setShowModal] = useState(false);
  const [windows, setWindows] = useState([]);
  const [topZ, setTopZ] = useState(10);
  const [time, setTime] = useState(new Date());

  useEffect(() => { var t = setInterval(() => setTime(new Date()), 1000); return () => clearInterval(t); }, []);
  useEffect(() => { if (!localStorage.getItem(API_KEY_STORAGE)) setShowModal(true); }, []);

  const openApp = app => {
    var ex = windows.find(w => w.appId === app.id);
    if (ex) { focusWindow(ex.id); return; }
    var id = Date.now(), offset = windows.length * 28;
    setWindows(ws => [...ws, {id, appId:app.id, pos:{x:80+offset, y:60+offset}, zIndex:topZ+1}]);
    setTopZ(z => z + 1);
  };

  const focusWindow = id => { setTopZ(z => z + 1); setWindows(ws => ws.map(w => w.id === id ? {...w, zIndex:topZ+1} : w)); };
  const closeWindow = id => setWindows(ws => ws.filter(w => w.id !== id));
  const handleSaveKey = k => { setApiKey(k); setShowModal(false); };
  const handleSkip = () => setShowModal(false);

  return (
    <div>
      {showModal && <ApiKeyModal onSave={handleSaveKey} onSkip={handleSkip}/>}
      <div style={{width:"100vw", height:"100vh", overflow:"hidden", background:"radial-gradient(ellipse at 30% 20%,#0D0830 0%,#050510 60%,#000008 100%)", fontFamily:"'Syne','DM Sans',system-ui,sans-serif", position:"relative"}}>

        <div style={{position:"absolute", width:500, height:500, borderRadius:"50%", background:"radial-gradient(circle,#7C3AED14 0%,transparent 70%)", top:-150, left:-150, pointerEvents:"none"}}/>
        <div style={{position:"absolute", width:350, height:350, borderRadius:"50%", background:"radial-gradient(circle,#0EA5E914 0%,transparent 70%)", bottom:80, right:-80, pointerEvents:"none"}}/>

        <div style={{position:"fixed", top:0, left:0, right:0, height:32, background:"rgba(8,8,18,0.9)", backdropFilter:"blur(20px)", borderBottom:"1px solid #1A1A2E", display:"flex", alignItems:"center", padding:"0 20px", zIndex:9999, justifyContent:"space-between"}}>
          <div style={{display:"flex", alignItems:"center", gap:6}}>
            <span style={{color:"#7C3AED", fontSize:14, fontWeight:800}}>◈</span>
            <span style={{color:"#E8E8F0", fontSize:11, fontWeight:700, letterSpacing:2}}>NEXOS</span>
          </div>
          <div style={{display:"flex", gap:16, alignItems:"center"}}>
            {windows.map(w => (
              <span key={w.id} onClick={() => focusWindow(w.id)} style={{color:"#8888A8", fontSize:11, cursor:"pointer"}}
                onMouseEnter={e => e.target.style.color = "#E8E8F0"} onMouseLeave={e => e.target.style.color = "#8888A8"}>
                {APPS[w.appId].icon} {APPS[w.appId].label}
              </span>
            ))}
          </div>
          <div style={{display:"flex", alignItems:"center", gap:12}}>
            {!apiKey && <button onClick={() => setShowModal(true)} style={{background:"none", border:"1px solid #7C3AED44", borderRadius:20, padding:"2px 10px", color:"#7C3AED", fontSize:10, cursor:"pointer"}}>+ IA</button>}
            {apiKey && <span style={{color:"#10B98166", fontSize:10}}>IA ativa</span>}
            <span style={{color:"#6668A0", fontSize:11}}>{time.toLocaleTimeString("pt-BR",{hour:"2-digit",minute:"2-digit"})}</span>
          </div>
        </div>

        <div style={{paddingTop:32, height:"100vh", position:"relative"}}>
          {windows.length === 0 && (
            <div style={{position:"absolute", top:"50%", left:"50%", transform:"translate(-50%,-50%)", textAlign:"center", animation:"fadeIn 0.6s ease"}}>
              <div style={{fontSize:56, marginBottom:12, animation:"float 4s ease-in-out infinite"}}>◈</div>
              <div style={{color:"#E8E8F0", fontSize:30, fontWeight:800, letterSpacing:-1}}>NEXOS OS</div>
              <div style={{color:"#555570", fontSize:13, marginTop:10, lineHeight:1.8}}>Seu sistema operacional pessoal.</div>
              {!apiKey && <div style={{marginTop:16}}><button onClick={() => setShowModal(true)} style={{background:"#7C3AED22", border:"1px solid #7C3AED44", borderRadius:20, padding:"6px 18px", color:"#7C3AED", fontSize:12, cursor:"pointer"}}>+ Ativar IA</button></div>}
            </div>
          )}
          {windows.map(w => (
            <Window key={w.id} app={APPS[w.appId]} apiKey={apiKey} onNeedKey={() => setShowModal(true)}
              onClose={() => closeWindow(w.id)} onFocus={() => focusWindow(w.id)}
              zIndex={w.zIndex} initialPos={w.pos}/>
          ))}
        </div>

        <div style={{position:"fixed", bottom:20, left:"50%", transform:"translateX(-50%)", background:"rgba(10,10,22,0.9)", backdropFilter:"blur(24px)", border:"1px solid #2A2A40", borderRadius:22, padding:"10px 18px", display:"flex", gap:10, zIndex:9999, boxShadow:"0 8px 40px rgba(0,0,0,0.6)"}}>
          {Object.values(APPS).map(app => {
            var open = windows.find(w => w.appId === app.id);
            return (
              <div key={app.id} style={{position:"relative"}}>
                <button onClick={() => openApp(app)} title={app.label}
                  style={{width:54, height:54, borderRadius:14, background: open ? app.color + "22" : "#141428", border:"1px solid " + (open ? app.color + "66" : "#2A2A40"), color: open ? app.color : "#8888A8", fontSize:22, cursor:"pointer", display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", gap:2, transition:"all 0.2s"}}
                  onMouseEnter={e => { e.currentTarget.style.transform = "scale(1.12) translateY(-5px)"; e.currentTarget.style.borderColor = app.color + "99"; }}
                  onMouseLeave={e => { e.currentTarget.style.transform = "scale(1)"; e.currentTarget.style.borderColor = open ? app.color + "66" : "#2A2A40"; }}>
                  {app.icon}
                  <span style={{fontSize:8, letterSpacing:0.5, fontWeight:700, color:"inherit", opacity:0.7}}>{app.label.toUpperCase()}</span>
                </button>
                {open && <div style={{position:"absolute", bottom:-6, left:"50%", transform:"translateX(-50%)", width:4, height:4, borderRadius:"50%", background:app.color}}/>}
              </div>
            );
          })}
        </div>
      </div>
    </div>
  );
}

ReactDOM.createRoot(document.getElementById("root")).render(<NexosOS/>);
</script>
</body>
</html>
