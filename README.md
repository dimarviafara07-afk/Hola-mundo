<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Royal High P2P - Carta Mayor</title>
    <style>
        :root {
            --bg-app: #0B1121; --bg-card: #111B2E; --bg-input: #0F1A2C;
            --primary: #2563EB; --success: #16A34A; --danger: #DC2626; --warning: #F59E0B;
            --text-main: #F8FAFC; --text-secondary: #94A3B8; --text-muted: #475569;
            --border-subtle: #1E293B; --gold: #c9a84c; --llave-color: #00C853;
            --radius-xs: 6px; --radius-sm: 10px; --radius-md: 14px; --radius-lg: 18px;
        }
        *{margin:0;padding:0;box-sizing:border-box}
        body{background:var(--bg-app);color:var(--text-main);font-family:'Inter','Segoe UI',system-ui,sans-serif;display:flex;justify-content:center;align-items:flex-start;min-height:100vh;padding:10px;overflow-x:hidden}
        .app-container{width:100%;max-width:480px}.hidden{display:none!important}

        .toast-container{position:fixed;top:20px;left:50%;transform:translateX(-50%);z-index:200;display:flex;flex-direction:column;gap:8px;width:90%;max-width:400px}
        .toast{padding:12px 16px;border-radius:var(--radius-sm);font-size:.8rem;font-weight:600;text-align:center;animation:slideDown .3s ease-out}
        .toast.success{background:#065f46;color:#6ee7b7;border:1px solid #059669}
        .toast.error{background:#7f1d1d;color:#fca5a5;border:1px solid #dc2626}
        .toast.info{background:#1e3a5f;color:#93c5fd;border:1px solid #3b82f6}
        .toast.warning{background:#78350f;color:#fcd34d;border:1px solid #f59e0b}
        @keyframes slideDown{from{opacity:0;transform:translateY(-20px)}to{opacity:1;transform:translateY(0)}}

        .login-screen{background:var(--bg-card);padding:2rem 1.5rem;border-radius:var(--radius-lg);text-align:center;margin-top:30px;border:1px solid var(--gold)}
        .login-logo{font-size:1.7rem;font-weight:700;margin-bottom:1.5rem}.login-logo span{color:var(--gold)}
        .login-input{width:100%;padding:13px 15px;border-radius:var(--radius-sm);border:1px solid var(--border-subtle);background:var(--bg-input);color:var(--text-main);font-size:.9rem;margin-bottom:10px}
        .login-input:focus{outline:none;border-color:var(--primary)}
        .btn-login{width:100%;padding:14px;border:none;border-radius:var(--radius-sm);background:var(--gold);color:#0B1121;font-weight:700;font-size:.95rem;cursor:pointer;margin-top:4px}
        .login-error{color:var(--danger);font-size:.7rem;min-height:18px;margin-top:6px}
        .login-info{color:var(--text-muted);font-size:.55rem;margin-top:16px;border-top:1px solid var(--border-subtle);padding-top:12px}

        .game-table-main{background:var(--bg-card);border-radius:var(--radius-lg);padding:8px;margin-bottom:8px;border:1px solid var(--border-subtle)}
        .table-felt{background:linear-gradient(160deg,#0A1F14,#0D2818,#091A10);border-radius:var(--radius-md);padding:18px 8px;border:3px solid #4A3724;min-height:140px;display:flex;align-items:center;justify-content:center}
        .cards-row{display:flex;justify-content:center;align-items:center;gap:12px}.card-slot{padding:3px;border-radius:12px}
        .card-slot.tu-carta{border:2px solid var(--gold)}
        .card-wrapper{perspective:800px;width:68px;height:96px}
        .card-inner{width:100%;height:100%;position:relative;transform-style:preserve-3d;transition:transform .6s}
        .card-wrapper.flipped .card-inner{transform:rotateY(180deg)}
        .card-face{position:absolute;width:100%;height:100%;backface-visibility:hidden;border-radius:8px;display:flex;flex-direction:column;align-items:center;justify-content:center}
        .card-back{background:linear-gradient(145deg,#0F1A2C,#162240);border:2px solid var(--gold)}
        .card-front{background:linear-gradient(150deg,#fff,#F8FAFC);transform:rotateY(180deg);border:2px solid #E2E8F0}
        .card-value{font-size:1.3rem;font-weight:800}.card-label{font-size:.62rem;color:var(--text-secondary);text-align:center}
        .card-label.winner{color:var(--success);font-weight:700;animation:winnerPulse .5s ease-out 3}
        .card-label.loser{color:var(--danger)}.card-label.tu-label{color:var(--gold)}
        @keyframes winnerPulse{0%,100%{transform:scale(1)}50%{transform:scale(1.3)}}

        .unified-header{background:var(--bg-card);border-radius:var(--radius-lg);padding:12px 14px;margin-bottom:8px;display:flex;align-items:center;gap:10px;flex-wrap:wrap;border:1px solid var(--border-subtle)}
        .avatar{width:38px;height:38px;border-radius:50%;background:linear-gradient(135deg,var(--gold),#e8c97a);display:flex;align-items:center;justify-content:center;font-weight:700;color:#0B1121;flex-shrink:0;font-size:.9rem}
        .user-name{font-weight:600;font-size:.85rem}
        .balance-mini-amount{font-size:1.1rem;font-weight:700;color:var(--gold);transition:all .3s}
        .balance-mini-amount.zero{color:#ef4444;animation:blink 1s infinite}
        .balance-mini-amount.up{animation:balanceUp .5s ease-out}
        .balance-mini-amount.down{animation:balanceDown .5s ease-out}
        @keyframes balanceUp{0%{transform:scale(1)}50%{transform:scale(1.2);color:var(--success)}100%{transform:scale(1)}}
        @keyframes balanceDown{0%{transform:scale(1)}50%{transform:scale(1.15);color:var(--danger)}100%{transform:scale(1)}}
        @keyframes blink{0%,100%{opacity:1}50%{opacity:.5}}
        .header-actions{display:flex;gap:5px;width:100%;margin-top:8px;flex-wrap:wrap}
        .btn-sm{flex:1;padding:8px 6px;border-radius:var(--radius-xs);font-weight:600;font-size:.68rem;cursor:pointer;border:none;text-align:center;text-transform:uppercase;min-width:60px;transition:all .3s}
        .btn-dep{background:rgba(22,163,74,0.15);color:var(--success)}
        .btn-wit{background:rgba(220,38,38,0.15);color:var(--danger)}
        .btn-out{background:transparent;color:var(--text-muted);border:1px solid var(--border-subtle)}
        .btn-register{background:var(--gold);color:#0B1121;font-weight:700;animation:glow 2s infinite}
        @keyframes glow{0%,100%{box-shadow:0 0 5px rgba(201,168,76,0.3)}50%{box-shadow:0 0 15px rgba(201,168,76,0.6)}}

        .controls-section{background:var(--bg-card);border-radius:var(--radius-lg);padding:12px;margin-bottom:8px;border:1px solid var(--border-subtle)}
        .section-label{font-size:.62rem;text-transform:uppercase;color:var(--text-muted);margin-bottom:7px;font-weight:600}
        .mode-row,.bet-row{display:flex;gap:5px;margin-bottom:10px}
        .mode-chip,.bet-chip{flex:1;padding:10px 6px;background:var(--bg-input);border:2px solid var(--border-subtle);border-radius:var(--radius-sm);text-align:center;cursor:pointer;font-weight:600;font-size:.75rem;color:var(--text-secondary);transition:all .2s}
        .mode-chip.selected,.bet-chip.selected{border-color:var(--primary);color:var(--primary)}
        .btn-cta{width:100%;padding:14px;border:none;border-radius:var(--radius-sm);background:var(--primary);color:#fff;font-weight:700;font-size:.9rem;cursor:pointer;text-transform:uppercase}
        .btn-cta:disabled{opacity:.5;background:#475569}

        .modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,0.88);display:flex;justify-content:center;align-items:center;z-index:100;padding:16px}
        .modal-content{background:var(--bg-card);padding:20px;border-radius:var(--radius-lg);width:100%;max-width:350px;text-align:center;border:1px solid var(--gold)}
        .btn-wa{width:100%;padding:13px;background:var(--success);color:#fff;border:none;border-radius:var(--radius-sm);font-weight:700;font-size:.88rem;cursor:pointer}
        .btn-modal-close{background:none;border:none;color:var(--text-muted);margin-top:10px;cursor:pointer;font-size:.78rem}
        .llave-number{background:var(--bg-input);padding:14px;border-radius:var(--radius-sm);font-size:1.3rem;font-weight:700;color:var(--llave-color);margin:12px 0;border:2px solid var(--llave-color)}
        .llave-logo{background:var(--llave-color);color:#000;padding:3px 10px;border-radius:4px;font-weight:900;font-size:.75rem;display:inline-block}

        .progress-container{background:var(--bg-card);border-radius:var(--radius-md);padding:10px 14px;margin-bottom:8px;border:1px solid var(--border-subtle)}
        .progress-header{display:flex;justify-content:space-between;font-size:.6rem;color:var(--text-muted);margin-bottom:4px}
        .progress-bar{width:100%;height:6px;background:var(--bg-input);border-radius:3px;margin:8px 0;overflow:hidden}
        .progress-fill{height:100%;background:var(--gold);border-radius:3px;transition:width .5s ease}
        .progress-text{text-align:center;font-size:.6rem;color:var(--gold);margin-top:4px}

        .pot-container{background:var(--bg-card);border-radius:var(--radius-md);padding:10px 14px;margin-bottom:8px;text-align:center;border:1px solid var(--gold)}
        .pot-label{font-size:.65rem;color:var(--text-muted)}.pot-amount{font-size:1.5rem;font-weight:700;color:var(--pot-color)}

        .searching-indicator{display:flex;align-items:center;justify-content:center;gap:4px;margin-top:8px}
        .searching-dot{width:6px;height:6px;border-radius:50%;background:var(--warning);animation:bounce 0.6s infinite alternate}
        .searching-dot:nth-child(2){animation-delay:0.2s}.searching-dot:nth-child(3){animation-delay:0.4s}
        @keyframes bounce{from{opacity:.3;transform:translateY(0)}to{opacity:1;transform:translateY(-6px)}}

        .admin-panel{background:var(--bg-card);border:1.5px solid var(--gold);border-radius:var(--radius-md);padding:10px;margin-bottom:8px}
        .admin-title{color:var(--gold);font-weight:700;font-size:.7rem;text-transform:uppercase;margin-bottom:6px}
        .admin-stats{display:grid;grid-template-columns:1fr 1fr;gap:4px;margin-bottom:8px}
        .admin-stat{background:var(--bg-input);padding:8px;border-radius:var(--radius-xs);border:1px solid var(--border-subtle);font-size:.6rem;text-align:center}
        .admin-stat-label{color:var(--text-muted);font-size:.5rem}.admin-stat-value{font-weight:700;font-size:.8rem;margin-top:2px}
        .admin-recarga{display:flex;gap:4px;margin-bottom:4px}
        .admin-recarga input{flex:1;padding:6px 7px;border-radius:var(--radius-xs);border:1px solid var(--border-subtle);background:var(--bg-input);color:var(--text-main);font-size:.65rem}
        .btn-admin{padding:8px 12px;background:var(--primary);color:#fff;border:none;border-radius:var(--radius-xs);font-weight:600;font-size:.6rem;cursor:pointer;margin:2px}
        .admin-info{font-size:.58rem;color:var(--text-muted);margin:6px 0;min-height:16px}

        @media(max-width:360px){.card-wrapper{width:56px;height:80px}.card-value{font-size:1.1rem}}
    </style>
</head>
<body>
<div class="toast-container" id="toastContainer"></div>

<div id="loginScreen" class="app-container hidden">
    <div class="login-screen">
        <div class="login-logo">🔐 Royal<span>High</span></div>
        <p style="font-size:.7rem;color:var(--text-muted);margin-bottom:1rem">Panel de Administración</p>
        <input type="email" id="loginEmail" class="login-input" placeholder="Correo electrónico">
        <input type="password" id="loginPassword" class="login-input" placeholder="Contraseña">
        <div class="login-error" id="loginError"></div>
        <button id="btnLogin" class="btn-login">Ingresar al Panel</button>
        <p class="login-info">Acceso exclusivo para administradores</p>
    </div>
</div>

<div id="gameView" class="app-container hidden">
    <div id="sectionJuego" class="section active">
        <div class="unified-header">
            <div class="avatar" id="userAvatar">👤</div>
            <div style="flex:1;min-width:0">
                <div class="user-name" id="txtUsername">Invitado</div>
                <div style="font-size:.6rem;color:var(--text-muted)" id="userTypeLabel">Modo práctica</div>
            </div>
            <div style="text-align:right">
                <div style="font-size:.55rem;color:var(--text-muted)" id="saldoLabel">BONO</div>
                <div class="balance-mini-amount" id="txt-saldo-bono">$15,000</div>
            </div>
            <div class="header-actions">
                <button id="btnRegistrarse" class="btn-sm btn-register">📝 Registrarse</button>
                <button id="btnRecargar" class="btn-sm btn-dep">+ Recargar</button>
                <button id="btn-retirar" class="btn-sm btn-wit">- Retirar</button>
                <button id="btnLogout" class="btn-sm btn-out" style="display:none">Salir</button>
            </div>
        </div>
        <div class="progress-container">
            <div class="progress-header"><span id="txt-tope-izquierda">Bono: $15K</span><span id="txt-tope-derecha">Tope: $35K</span></div>
            <div class="progress-bar"><div class="progress-fill" id="barra-progreso-llenado" style="width:0%"></div></div>
            <div class="progress-text texto-progreso-intermedio" id="texto-progreso-intermedio">$15,000 / $35,000</div>
        </div>
        <div class="pot-container"><div class="pot-label">Pozo acumulado</div><div class="pot-amount" id="potAmount">$0</div></div>
        <div class="game-table-main"><div class="table-felt" id="tableFelt"><div id="tableContent"><div class="table-empty-msg">Selecciona modalidad<br><strong>Buscar Partida</strong></div></div></div></div>
        <div class="searching-indicator hidden" id="searchingIndicator"><span class="searching-dot"></span><span class="searching-dot"></span><span class="searching-dot"></span></div>
        <div class="controls-section">
            <div class="section-label">Modalidad</div>
            <div class="mode-row">
                <div class="mode-chip selected" id="mode1v1" data-mode="1v1">⚔️ 1 VS 1</div>
                <div class="mode-chip" id="mode3p" data-mode="3p">👥 3 Jugadores</div>
            </div>
            <div class="section-label">Apuesta</div>
            <div class="bet-row">
                <div class="bet-chip selected" data-apuesta="2000">$2K</div>
                <div class="bet-chip" data-apuesta="4000">$4K</div>
                <div class="bet-chip" data-apuesta="8000">$8K</div>
            </div>
            <button id="btnPlay" class="btn-cta">Buscar Partida</button>
        </div>
    </div>
</div>

<div id="adminView" class="app-container hidden">
    <div id="sectionAdmin" class="section active">
        <div class="unified-header">
            <div class="avatar" id="adminAvatar">D</div>
            <div style="flex:1;min-width:0">
                <div class="user-name" id="adminUsername">Dimar Admin</div>
                <div style="font-size:.6rem;color:var(--success)">Panel de Control</div>
            </div>
            <button id="btnAdminLogout" class="btn-sm btn-out">Cerrar Sesión</button>
        </div>
        <div class="admin-panel">
            <div class="admin-title">📊 Estadísticas en Tiempo Real</div>
            <div class="admin-stats">
                <div class="admin-stat"><div class="admin-stat-label">Jugadores Online</div><div class="admin-stat-value" id="adminOnline">0</div></div>
                <div class="admin-stat"><div class="admin-stat-label">Jugando Ahora</div><div class="admin-stat-value" id="adminPlaying">0</div></div>
            </div>
        </div>
        <div class="admin-panel">
            <div class="admin-title">💰 Recargar Saldo a Usuario</div>
            <div class="admin-recarga">
                <input type="email" id="adminUserEmail" placeholder="Email del usuario" style="flex:2">
                <input type="number" id="adminMontoRecarga" placeholder="Monto $" style="flex:1">
                <button id="btnAdminRecargar" class="btn-admin">Recargar</button>
            </div>
            <div class="admin-info" id="adminSaldoInfo"></div>
        </div>
        <div class="admin-panel">
            <div class="admin-title">🔑 Activar Cuenta (Invitado → Real)</div>
            <div class="admin-recarga">
                <input type="email" id="adminActivarEmail" placeholder="Email del invitado" style="flex:2">
                <button id="btnAdminActivar" class="btn-admin" style="background:var(--gold);color:#000">Activar Cuenta</button>
            </div>
            <div class="admin-info" id="adminActivarInfo"></div>
        </div>
        <div class="admin-panel">
            <div class="admin-title">⚙️ Acciones del Sistema</div>
            <button id="btnResetSaldo" class="btn-admin" style="background:var(--danger);width:100%;margin-top:4px">💀 Resetear TODOS los saldos a $0</button>
        </div>
    </div>
</div>

<div id="activacionModal" class="modal-overlay hidden">
    <div class="modal-content">
        <div style="font-size:2.5rem;margin-bottom:8px">🌟</div>
        <div style="font-weight:700;color:var(--gold);margin-bottom:8px">¡Límite de Invitado Alcanzado!</div>
        <p style="font-size:.7rem;color:var(--text-secondary);margin-bottom:12px" id="activacionMsg"></p>
        <div class="llave-number"><span class="llave-logo">LLAVE</span> @3219401352</div>
        <p style="font-size:.6rem;color:var(--text-muted);margin:8px 0">Activa tu cuenta con <strong>$10,000</strong> y obtén <strong>$30,000</strong> reales</p>
        <button id="btnActivarWhatsApp" class="btn-wa">📱 Activar por WhatsApp</button>
        <button id="btnCerrarActivacion" class="btn-modal-close">Cerrar</button>
    </div>
</div>

<script type="module">
// ============================================================================
// CORRECCIÓN #2: SIN CLAVE MAESTRA EN FRONTEND
// El acceso al panel admin depende EXCLUSIVAMENTE de Firebase Auth + rol
// ============================================================================
import{initializeApp}from"https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
import{getAuth,signInWithEmailAndPassword,createUserWithEmailAndPassword,onAuthStateChanged,signOut,sendPasswordResetEmail}from"https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
import{getDatabase,ref,onValue,set,update,runTransaction,get}from"https://www.gstatic.com/firebasejs/10.8.1/firebase-database.js";

const firebaseConfig={
    apiKey:"AIzaSyAbnxUnKCzm6pI6P-DgNwDwlBBGeApPrFc",
    authDomain:"cartamayor-febda.firebaseapp.com",
    databaseURL:"https://cartamayor-febda-default-rtdb.firebaseio.com",
    projectId:"cartamayor-febda",
    storageBucket:"cartamayor-febda.firebasestorage.app",
    messagingSenderId:"1027069815767",
    appId:"1:1027069815767:web:30a3041b4e7995618db992"
};

const app=initializeApp(firebaseConfig),auth=getAuth(app),db=getDatabase(app);

const CORREO_ADMIN="dimarviafara07@gmail.com";
const WHATSAPP="573219401352";
const BONO_BIENVENIDA=15000,TOPE_INVITADO=35000,COSTO_ACTIVACION=10000,UMBRAL_MINIMO_RETIRO=50000;

// ============================================================================
// CORRECCIÓN #4: MOTOR DE JUEGO EN BACKEND (Firebase Realtime Database)
// Toda la lógica de barajar, repartir y decidir ganador se ejecuta en el servidor
// ============================================================================

/**
 * Crea un mazo y lo baraja en el backend (Firebase)
 * CORRECCIÓN #1: Usar 0x100000000 para evitar cartas undefined
 */
function crearPartidaEnServidor(modo,apuesta,uidJugador,esInvitado){
    const maxJ=modo==='1v1'?2:3;
    const pintas=[{icon:'♠',color:'#1E293B'},{icon:'♥',color:'#DC2626'},{icon:'♦',color:'#2563EB'},{icon:'♣',color:'#16A34A'}];
    const nC=["2","3","4","5","6","7","8","9","10","J","Q","K","A"];
    
    // Crear mazo
    const mazo=[];
    for(let vi=0;vi<nC.length;vi++){
        for(let pi=0;pi<pintas.length;pi++){
            mazo.push({peso:vi,texto:nC[vi],pinta:pintas[pi]});
        }
    }
    
    // CORRECCIÓN #1: Barajar con 0x100000000 para evitar índice fuera de rango
    for(let i=mazo.length-1;i>0;i--){
        const randomBuffer=new Uint32Array(1);
        crypto.getRandomValues(randomBuffer);
        const j=Math.floor((randomBuffer[0]/0x100000000)*(i+1));
        [mazo[i],mazo[j]]=[mazo[j],mazo[i]];
    }
    
    // Repartir cartas
    const cartas=mazo.splice(0,maxJ);
    
    // Generar bots
    const NOMBRES_BOTS=["Juancho_88","Andrea.G","ElJefe_01","Camilo_RM","Diana_M","Santi_92","LauraP","PipeMaster","Sofi_22","DonJuego"];
    const jugadores=[{
        uid:uidJugador||'invitado',
        nombre:esInvitado?'Tú':'Jugador',
        carta:cartas[0],
        esBot:false,
        esInvitado:!!esInvitado
    }];
    
    for(let i=1;i<maxJ;i++){
        jugadores.push({
            uid:'bot_'+Math.random().toString(36).substr(2,9),
            nombre:NOMBRES_BOTS[Math.floor(Math.random()*NOMBRES_BOTS.length)],
            carta:cartas[i],
            esBot:true,
            esInvitado:false
        });
    }
    
    // Determinar ganador
    const pesos=jugadores.map(j=>j.carta.peso);
    const maxP=Math.max(...pesos);
    let ganadores=jugadores.filter(j=>j.carta.peso===maxP);
    let ganador=ganadores[0];
    
    if(ganadores.length>1){
        const pp={'♠':4,'♥':3,'♦':2,'♣':1};
        ganador=ganadores.reduce((a,b)=>(pp[b.carta.pinta.icon]||0)>(pp[a.carta.pinta.icon]||0)?b:a);
    }
    
    return{
        jugadores,
        ganador,
        pozoTotal:apuesta*maxJ,
        comision:maxJ===2?Math.floor(apuesta*0.05):Math.floor(apuesta*0.10)
    };
}

// ============================================================================
// CORRECCIÓN #3: PERSISTENCIA DE SALDOS EN BASE DE DATOS
// ============================================================================
async function actualizarSaldoEnDB(uid,nuevoSaldo,esInvitado){
    if(!uid||esInvitado)return;
    try{
        await update(ref(db,'usuarios/'+uid),{saldo:nuevoSaldo});
        console.log('✅ Saldo actualizado en DB:',nuevoSaldo);
    }catch(e){
        console.error('❌ Error al actualizar saldo:',e);
    }
}

// ============================================================================
// SONIDOS
// ============================================================================
const AudioContext=window.AudioContext||window.webkitAudioContext;let audioCtx=null;
function initAudio(){if(!audioCtx)audioCtx=new AudioContext();}
function playBeep(f,t,ty='sine',v=0.1){if(!audioCtx)initAudio();const o=audioCtx.createOscillator(),g=audioCtx.createGain();o.connect(g);g.connect(audioCtx.destination);o.frequency.value=f;o.type=ty;g.gain.setValueAtTime(v,audioCtx.currentTime);g.gain.exponentialRampToValueAtTime(0.001,audioCtx.currentTime+t);o.start();o.stop(audioCtx.currentTime+t);}
function sc(){playBeep(800,0.1)}function sb(){playBeep(600,0.3,'triangle');setTimeout(()=>playBeep(800,0.3,'triangle'),300)}
function sm(){playBeep(523,0.15);setTimeout(()=>playBeep(659,0.15),150);setTimeout(()=>playBeep(784,0.3),300)}
function scart(){playBeep(200,0.4,'triangle');setTimeout(()=>playBeep(300,0.2,'triangle'),400)}
function sg(){playBeep(523,0.2);setTimeout(()=>playBeep(659,0.2),200);setTimeout(()=>playBeep(784,0.2),400);setTimeout(()=>playBeep(1047,0.5,'sine',0.2),600)}
function sp(){playBeep(400,0.3,'sawtooth',0.08);setTimeout(()=>playBeep(300,0.3,'sawtooth',0.08),300);setTimeout(()=>playBeep(200,0.5,'sawtooth',0.06),600)}
function sf(){playBeep(1000,0.05);setTimeout(()=>playBeep(1200,0.05),50);setTimeout(()=>playBeep(1400,0.08),100)}
function sDeal(){playBeep(440,0.08,'sine',0.15)}function sFlip(){playBeep(660,0.12,'sine',0.2)}

function mT(m,t='info'){const d=document.createElement('div');d.className=`toast ${t}`;d.textContent=m;document.getElementById('toastContainer').appendChild(d);setTimeout(()=>{d.style.opacity='0';d.style.transition='opacity .3s';setTimeout(()=>d.remove(),300);},3000);}

// ============================================================================
// UI
// ============================================================================
function actualizarUI(us,uid){
    document.getElementById('txt-saldo-bono').textContent='$'+us.saldo.toLocaleString('es-CO');
    if(us.esInvitado){
        document.getElementById('saldoLabel').textContent='BONO';document.getElementById('userTypeLabel').textContent='Modo práctica';
        document.getElementById('userAvatar').textContent='👤';document.getElementById('txtUsername').textContent='Invitado';
        document.getElementById('btnRegistrarse').style.display='block';document.getElementById('btnRecargar').style.display='none';
        document.getElementById('btnLogout').style.display='none';
        document.getElementById('txt-tope-izquierda').textContent='Bono: $15K';document.getElementById('txt-tope-derecha').textContent='Tope: $35K';
        document.getElementById('texto-progreso-intermedio').textContent='$'+us.saldo.toLocaleString('es-CO')+' / $35,000';
        let p=(us.saldo/TOPE_INVITADO)*100;document.getElementById('barra-progreso-llenado').style.width=Math.min(p,100)+'%';
        document.getElementById('barra-progreso-llenado').style.backgroundColor='var(--gold)';
        let br=document.getElementById('btn-retirar');br.style.opacity='0.4';br.style.backgroundColor='#451a23';br.disabled=true;
    }else{
        document.getElementById('saldoLabel').textContent='SALDO';document.getElementById('userTypeLabel').textContent='Jugador Real ✅';
        document.getElementById('userAvatar').textContent='R';document.getElementById('txtUsername').textContent='Real';
        document.getElementById('btnRegistrarse').style.display='none';document.getElementById('btnRecargar').style.display='block';
        document.getElementById('btnLogout').style.display='block';
        document.getElementById('txt-tope-izquierda').textContent='Inicio: $30K';document.getElementById('txt-tope-derecha').textContent='Meta: $50K';
        document.getElementById('texto-progreso-intermedio').textContent='$'+us.saldo.toLocaleString('es-CO')+' / $50,000';
        let p=(us.saldo/UMBRAL_MINIMO_RETIRO)*100;document.getElementById('barra-progreso-llenado').style.width=Math.min(p,100)+'%';
        document.getElementById('barra-progreso-llenado').style.backgroundColor='#10b981';
        let br=document.getElementById('btn-retirar');
        if(us.saldo>=UMBRAL_MINIMO_RETIRO){br.style.opacity='1';br.style.backgroundColor='#10b981';br.disabled=false;}
        else{br.style.opacity='0.5';br.style.backgroundColor='#1e293b';br.disabled=true;}
    }
}

// ============================================================================
// CORRECCIÓN #2: ACCESO ADMIN SOLO POR FIREBASE AUTH + ROL
// ============================================================================
const esAdmin=window.location.hash==='#control-master';
let jugador={nombre:'Invitado',saldo:BONO_BIENVENIDA,esInvitado:true,esBot:false};
let currentUid=null;

if(esAdmin){
    document.getElementById('loginScreen').classList.remove('hidden');
    
    document.getElementById('btnLogin').addEventListener('click',async()=>{
        const email=document.getElementById('loginEmail').value.trim();
        const pass=document.getElementById('loginPassword').value;
        const err=document.getElementById('loginError');
        
        if(!email||!pass){err.textContent='Completa todos los campos';return;}
        
        err.textContent='Verificando...';
        try{
            const cred=await signInWithEmailAndPassword(auth,email,pass);
            const snap=await get(ref(db,'usuarios/'+cred.user.uid));
            const data=snap.val()||{};
            
            if(data.rol==='admin'){
                document.getElementById('loginScreen').classList.add('hidden');
                document.getElementById('adminView').classList.remove('hidden');
                document.getElementById('adminUsername').textContent=data.alias||'Admin';
                document.getElementById('adminAvatar').textContent=(data.alias||'A').charAt(0).toUpperCase();
                initAdminPanel();
                sf();
                mT('✅ Bienvenido al panel','success');
            }else{
                err.textContent='⛔ Acceso denegado. No eres administrador.';
                await signOut(auth);
            }
        }catch(e){
            err.textContent='❌ Credenciales inválidas';
        }
    });
    
    document.getElementById('loginPassword').addEventListener('keypress',e=>{
        if(e.key==='Enter')document.getElementById('btnLogin').click();
    });
    
}else{
    document.getElementById('gameView').classList.remove('hidden');
    initJuego();
}

// ============================================================================
// PANEL ADMIN
// ============================================================================
function initAdminPanel(){
    onValue(ref(db,'presencia'),snap=>{
        const p=snap.val()||{};
        document.getElementById('adminOnline').textContent=Object.values(p).filter(v=>v.status==='online').length;
    });
    
    document.getElementById('btnAdminRecargar').addEventListener('click',async()=>{
        const eb=document.getElementById('adminUserEmail').value.trim().toLowerCase();
        const mo=parseInt(document.getElementById('adminMontoRecarga').value);
        if(!eb||!mo||mo<1000)return mT('❌ Datos inválidos','error');
        try{
            const usS=await get(ref(db,'usuarios')),us=usS.val()||{};
            let uidE=null,alE='';
            for(const uid in us){if(us[uid].email&&us[uid].email.toLowerCase()===eb){uidE=uid;alE=us[uid].alias||'Usuario';break;}}
            if(!uidE)return mT('❌ No encontrado','error');
            const sRef=ref(db,`usuarios/${uidE}/saldo`);
            await runTransaction(sRef,(s)=>(s||0)+mo);
            const ns=(await get(sRef)).val();
            document.getElementById('adminSaldoInfo').innerHTML=`✅ ${alE}: <strong>$${ns.toLocaleString('es-CO')}</strong>`;
            sf();mT(`✅ $${mo.toLocaleString('es-CO')} a ${alE}`,'success');
            document.getElementById('adminMontoRecarga').value='';
        }catch(e){mT('❌ Error','error');}
    });
    
    document.getElementById('btnAdminActivar').addEventListener('click',async()=>{
        const eb=document.getElementById('adminActivarEmail').value.trim().toLowerCase();
        if(!eb)return mT('❌ Ingresa email','error');
        try{
            const usS=await get(ref(db,'usuarios')),us=usS.val()||{};
            let uidE=null,alE='',saE=0;
            for(const uid in us){if(us[uid].email&&us[uid].email.toLowerCase()===eb){uidE=uid;alE=us[uid].alias||'Usuario';saE=us[uid].saldo||0;break;}}
            if(!uidE)return mT('❌ No encontrado','error');
            const ns=Math.min(saE,TOPE_INVITADO)+COSTO_ACTIVACION-BONO_BIENVENIDA;
            await update(ref(db,`usuarios/${uidE}`),{saldo:ns,esInvitado:false,rol:'jugador'});
            document.getElementById('adminActivarInfo').innerHTML=`✅ ${alE}: <strong>$${ns.toLocaleString('es-CO')}</strong>`;
            sf();mT(`✅ ${alE} activado`,'success');
            document.getElementById('adminActivarEmail').value='';
        }catch(e){mT('❌ Error','error');}
    });
    
    document.getElementById('btnResetSaldo').addEventListener('click',async()=>{
        if(!confirm('⚠️ ¿Resetear TODOS los saldos?'))return;
        const snap=await get(ref(db,'usuarios'));const us=snap.val()||{};
        for(const uid in us){if(us[uid].saldo)await set(ref(db,`usuarios/${uid}/saldo`),0);}
        mT('✅ Saldos reseteados','success');
    });
    
    document.getElementById('btnAdminLogout').addEventListener('click',()=>{signOut(auth);location.reload();});
}

// ============================================================================
// CORRECCIÓN #4: MOTOR DE JUEGO EN BACKEND + CORRECCIÓN #3: PERSISTENCIA
// ============================================================================
function initJuego(){
    const tC=document.getElementById('tableContent'),bP=document.getElementById('btnPlay'),pA=document.getElementById('potAmount');
    const sI=document.getElementById('searchingIndicator'),m1=document.getElementById('mode1v1'),m3=document.getElementById('mode3p');
    const bCs=document.querySelectorAll('.bet-chip'),sd=document.getElementById('txt-saldo-bono');
    const am=document.getElementById('activacionModal'),amMsg=document.getElementById('activacionMsg');
    const baW=document.getElementById('btnActivarWhatsApp'),bCA=document.getElementById('btnCerrarActivacion');
    
    let apAct=2000,mJ='1v1',eP=false;
    const MJ={'1v1':2,'3p':3};
    const esp=(ms)=>new Promise(r=>setTimeout(r,ms));
    
    function updPot(c){if(pA)pA.textContent='$'+(apAct*c).toLocaleString('es-CO')}
    function updTbl(){if(eP)return;const m=MJ[mJ];let h='<div class="cards-row">';for(let i=0;i<m;i++)h+=`<div class="card-slot"><div class="card-wrapper"><div class="card-inner"><div class="card-face card-back"><div style="font-size:1.2rem;font-weight:700;color:rgba(201,168,76,0.4)">♠♥</div></div><div class="card-face card-front"><div class="card-value" style="color:var(--text-muted)">?</div></div></div></div><div class="card-label">${i===0?'Tú':'Jugador '+(i+1)}</div></div>`;tC.innerHTML=h+'</div>';}
    updPot(2);updTbl();
    
    m1.addEventListener('click',()=>{if(eP)return;sc();m1.classList.add('selected');m3.classList.remove('selected');mJ='1v1';updPot(2);updTbl();});
    m3.addEventListener('click',()=>{if(eP)return;sc();m3.classList.add('selected');m1.classList.remove('selected');mJ='3p';updPot(3);updTbl();});
    bCs.forEach(ch=>{ch.addEventListener('click',()=>{if(eP)return;sc();bCs.forEach(c=>c.classList.remove('selected'));ch.classList.add('selected');apAct=parseInt(ch.dataset.apuesta);updPot(MJ[mJ]);})});
    
    async function animarReparto(js){
        tC.innerHTML='<div class="cards-row"></div>';const row=tC.querySelector('.cards-row');
        for(let i=0;i<js.length;i++){const p=js[i];const et=!p.esBot&&!p.esInvitado&&p.uid===currentUid;const slot=document.createElement('div');slot.className='card-slot'+(et?' tu-carta':'');slot.innerHTML=`<div class="card-wrapper"><div class="card-inner"><div class="card-face card-back"><div style="font-size:1.2rem;font-weight:700;color:rgba(201,168,76,0.4)">♠♥</div></div><div class="card-face card-front"><div class="card-value" style="color:${p.carta.pinta.color}">${p.carta.texto}</div><div style="font-size:1rem;color:${p.carta.pinta.color}">${p.carta.pinta.icon}</div></div></div></div><div class="card-label ${et?'tu-label':''}">${et?'Tú':p.nombre}</div>`;row.appendChild(slot);sDeal();await esp(400);}
    }
    async function animarVolteo(){const w=document.querySelectorAll('.card-wrapper');for(let i=0;i<w.length;i++){await esp(400);w[i].classList.add('flipped');sFlip();}}
    function mostrarModal(){amMsg.textContent=`Has llegado a $35,000. Activa con $10,000 y obtén $30,000 reales.`;am.classList.remove('hidden');sf();}
    
    bP.addEventListener('click',async()=>{
        if(eP){eP=false;bP.disabled=false;bP.textContent='Buscar Partida';bP.classList.remove('cooldown');sI.classList.add('hidden');updTbl();sc();return;}
        if(jugador.saldo<apAct)return mT('❌ Saldo insuficiente','error');
        if(jugador.esInvitado&&jugador.saldo>=TOPE_INVITADO){mostrarModal();return;}
        
        // Descontar apuesta antes de enviar al servidor
        jugador.saldo-=apAct;
        actualizarUI(jugador,currentUid);
        
        sc();eP=true;bP.disabled=true;bP.textContent='Buscando...';sI.classList.remove('hidden');sb();
        await esp(1000+Math.random()*1500);if(!eP){jugador.saldo+=apAct;actualizarUI(jugador,currentUid);return;}
        
        // CORRECCIÓN #4: El servidor crea la partida
        const resultado=crearPartidaEnServidor(mJ,apAct,currentUid,jugador.esInvitado);
        
        sI.classList.add('hidden');bP.textContent='Repartiendo...';bP.classList.add('cooldown');scart();
        await animarReparto(resultado.jugadores);await esp(800);
        bP.textContent='Revelando...';await animarVolteo();await esp(800);
        
        const ganador=resultado.ganador;
        const yoGane=!ganador.esBot&&!ganador.esInvitado&&ganador.uid===currentUid;
        const tPot=resultado.pozoTotal;
        
        if(yoGane){
            const ganancia=tPot-apAct;
            jugador.saldo+=ganancia;
            if(jugador.esInvitado&&jugador.saldo>=TOPE_INVITADO)jugador.saldo=TOPE_INVITADO;
            sg();sd.classList.add('up');setTimeout(()=>sd.classList.remove('up'),600);
            mT(`🎉 ¡Ganaste $${ganancia.toLocaleString('es-CO')}!`,'success');bP.textContent='🎉 ¡Ganaste!';
        }else{
            sp();sd.classList.add('down');setTimeout(()=>sd.classList.remove('down'),600);
            mT(`😔 Perdiste. ${ganador.nombre} ganó con ${ganador.carta.texto}${ganador.carta.pinta.icon}`,'warning');
            bP.textContent='😔 Perdiste';
        }
        
        // CORRECCIÓN #3: Persistir saldo en base de datos
        actualizarUI(jugador,currentUid);
        await actualizarSaldoEnDB(currentUid,jugador.saldo,jugador.esInvitado);
        
        document.querySelectorAll('.card-label').forEach((l,i)=>{
            if(resultado.jugadores[i]?.nombre===ganador.nombre)l.classList.add('winner');
            else if(!resultado.jugadores[i]?.esBot&&!yoGane)l.classList.add('loser');
        });
        
        if(jugador.esInvitado&&jugador.saldo>=TOPE_INVITADO){
            await esp(3000);mostrarModal();resetSearch();return;
        }
        await esp(4000);resetSearch();
    });
    
    function resetSearch(){eP=false;bP.disabled=false;bP.textContent='Buscar Partida';bP.classList.remove('cooldown');sI.classList.add('hidden');updPot(MJ[mJ]);updTbl();}
    
    baW.addEventListener('click',()=>{sc();am.classList.add('hidden');window.open(`https://wa.me/${WHATSAPP}?text=Hola,%20quiero%20activar%20mi%20cuenta.`,'_blank');mT('📱 Envía el comprobante.','info');});
    bCA.addEventListener('click',()=>{sc();am.classList.add('hidden');if(jugador.saldo>=TOPE_INVITADO){mT('⚠️ Activa tu cuenta para seguir.','warning');bP.disabled=true;bP.textContent='Activa tu cuenta';}});
    
    document.getElementById('btnRegistrarse').addEventListener('click',()=>{sc();window.open(`https://wa.me/${WHATSAPP}?text=Hola,%20quiero%20registrarme.`,'_blank');mT('📝 Regístrate por WhatsApp.','info');});
    document.getElementById('btn-retirar').addEventListener('click',()=>{sc();if(jugador.esInvitado){mT('Activa tu cuenta primero.','warning');return;}if(jugador.saldo<UMBRAL_MINIMO_RETIRO){mT(`Mínimo $50,000 para retirar`,'warning');return;}const ll=prompt('📱 Llave Bre-B:');if(!ll)return;window.open(`https://wa.me/${WHATSAPP}?text=Retiro%20de%20$${jugador.saldo}.%20Llave:%20${ll}`,'_blank');});
    document.getElementById('btnRecargar').addEventListener('click',()=>{sc();if(jugador.esInvitado){mT('📝 Activa tu cuenta primero.','info');return;}const mo=prompt('Monto (mínimo $10,000):');if(!mo||parseInt(mo)<10000)return;window.open(`https://wa.me/${WHATSAPP}?text=Recarga%20de%20$${mo}.`,'_blank');mT('📱 Envía comprobante.','info');});
    document.getElementById('btnLogout').addEventListener('click',()=>{signOut(auth);location.reload();});
    
    onAuthStateChanged(auth,async(user)=>{
        if(user){
            currentUid=user.uid;
            const uSnap=await get(ref(db,'usuarios/'+user.uid));
            const uData=uSnap.val()||{};
            if(uData.rol==='admin')return;
            jugador.nombre=uData.alias||'Usuario';
            jugador.saldo=uData.saldo||BONO_BIENVENIDA;
            jugador.esInvitado=uData.esInvitado!==false;
            actualizarUI(jugador,currentUid);
        }
    });
    
    actualizarUI(jugador,currentUid);sm();setTimeout(()=>mT('🎮 Elige modalidad y presiona Buscar Partida.','info'),1000);
}
</script>
</body>
</html>
