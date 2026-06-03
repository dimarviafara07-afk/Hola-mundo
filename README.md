<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Royal High P2P - Carta Mayor</title>
    <style>
        :root {
            --bg-app: #0B1121; --bg-card: #111B2E; --bg-card-hover: #162240; --bg-input: #0F1A2C;
            --primary: #2563EB; --primary-hover: #1D4ED8; --primary-soft: rgba(37,99,235,0.15);
            --success: #16A34A; --danger: #DC2626; --warning: #F59E0B; --text-main: #F8FAFC;
            --text-secondary: #94A3B8; --text-muted: #475569; --border-subtle: #1E293B;
            --gold: #c9a84c; --gold-soft: rgba(201,168,76,0.1); --pot-color: #d4a853;
            --radius-xs: 6px; --radius-sm: 10px; --radius-md: 14px; --radius-lg: 18px;
            --llave-color: #00C853;
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

        .game-table-main{background:var(--bg-card);border-radius:var(--radius-lg);padding:8px;margin-bottom:8px;border:1px solid var(--border-subtle)}
        .table-felt{background:linear-gradient(160deg,#0A1F14,#0D2818,#091A10);border-radius:var(--radius-md);padding:18px 8px;border:3px solid #4A3724;min-height:140px;display:flex;align-items:center;justify-content:center;position:relative;overflow:hidden}
        .cards-row{display:flex;justify-content:center;align-items:center;gap:12px}.card-slot{padding:3px;border-radius:12px}
        .card-slot.tu-carta{border:2px solid var(--gold);background:rgba(201,168,76,0.1)}
        .card-wrapper{perspective:800px;width:68px;height:96px}
        .card-inner{width:100%;height:100%;position:relative;transform-style:preserve-3d;transition:transform .6s cubic-bezier(.4,0,.2,1)}
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
        @keyframes balanceUp{0%{transform:scale(1);color:var(--gold)}50%{transform:scale(1.2);color:var(--success)}100%{transform:scale(1);color:var(--gold)}}
        @keyframes balanceDown{0%{transform:scale(1);color:var(--gold)}50%{transform:scale(1.15);color:var(--danger)}100%{transform:scale(1);color:var(--gold)}}
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
        .mode-chip.selected,.bet-chip.selected{border-color:var(--primary);background:var(--primary-soft);color:var(--primary)}
        .btn-cta{width:100%;padding:14px;border:none;border-radius:var(--radius-sm);background:var(--primary);color:#fff;font-weight:700;font-size:.9rem;cursor:pointer;text-transform:uppercase}
        .btn-cta:disabled{opacity:.5;background:#475569}

        .modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,0.88);display:flex;justify-content:center;align-items:center;z-index:100;padding:16px}
        .modal-content{background:var(--bg-card);padding:20px;border-radius:var(--radius-lg);width:100%;max-width:350px;text-align:center;border:1px solid var(--gold)}
        .btn-wa{width:100%;padding:13px;background:var(--success);color:#fff;border:none;border-radius:var(--radius-sm);font-weight:700;font-size:.88rem;cursor:pointer}
        .btn-modal-close{background:none;border:none;color:var(--text-muted);margin-top:10px;cursor:pointer;font-size:.78rem}
        .llave-number{background:var(--bg-input);padding:14px;border-radius:var(--radius-sm);font-size:1.3rem;font-weight:700;color:var(--llave-color);margin:12px 0;border:2px solid var(--llave-color)}
        .llave-logo{background:var(--llave-color);color:#000;padding:3px 10px;border-radius:4px;font-weight:900;font-size:.75rem;display:inline-block}

        .retiro-step{display:flex;align-items:center;gap:8px;padding:8px 12px;margin:6px 0;border-radius:var(--radius-sm);font-size:.7rem;text-align:left}
        .retiro-step.bloqueado{background:rgba(220,38,38,0.1);border:1px solid rgba(220,38,38,0.2);color:var(--danger)}
        .retiro-step.activo{background:rgba(22,163,74,0.1);border:1px solid rgba(22,163,74,0.2);color:var(--success)}
        .retiro-step.pendiente{background:rgba(245,158,11,0.1);border:1px solid rgba(245,158,11,0.2);color:var(--warning)}

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
        .admin-stats{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:4px;margin-bottom:8px}
        .admin-stat{background:var(--bg-input);padding:6px;border-radius:var(--radius-xs);border:1px solid var(--border-subtle);font-size:.6rem;text-align:center}
        .admin-stat-label{color:var(--text-muted);font-size:.5rem}.admin-stat-value{font-weight:700;font-size:.75rem;margin-top:1px}
        .admin-recarga{display:flex;gap:4px;margin-bottom:4px}
        .admin-recarga input{flex:1;padding:6px 7px;border-radius:var(--radius-xs);border:1px solid var(--border-subtle);background:var(--bg-input);color:var(--text-main);font-size:.65rem;min-width:0}
        .btn-admin-recargar{padding:6px 8px;background:var(--primary);color:#fff;border:none;border-radius:var(--radius-xs);font-weight:600;font-size:.6rem;cursor:pointer}
        .btn-admin-ver{padding:6px 8px;background:var(--warning);color:#000;border:none;border-radius:var(--radius-xs);font-weight:600;font-size:.55rem;cursor:pointer}
        .admin-saldo-info{font-size:.58rem;color:var(--text-muted);margin-bottom:6px;min-height:16px}
        .btn-admin-action{padding:6px 8px;background:rgba(220,38,38,0.1);color:var(--danger);border:1px solid rgba(220,38,38,0.2);border-radius:var(--radius-xs);font-weight:600;font-size:.55rem;cursor:pointer;text-align:center;margin:2px}
        .login-screen{background:var(--bg-card);padding:2rem 1.5rem;border-radius:var(--radius-lg);text-align:center;margin-top:30px;border:1px solid var(--gold)}
        .login-logo{font-size:1.7rem;font-weight:700;margin-bottom:1.5rem}.login-logo span{color:var(--gold)}
        .login-input{width:100%;padding:13px 15px;border-radius:var(--radius-sm);border:1px solid var(--border-subtle);background:var(--bg-input);color:var(--text-main);font-size:.9rem;margin-bottom:10px}
        .login-input:focus{outline:none;border-color:var(--primary)}
        .btn-login{width:100%;padding:14px;border:none;border-radius:var(--radius-sm);background:var(--gold);color:#0B1121;font-weight:700;font-size:.95rem;cursor:pointer;margin-top:4px}
        .login-error{color:var(--danger);font-size:.7rem;min-height:18px;margin-top:6px}
        .forgot-link{display:block;margin-top:12px;font-size:.7rem;color:var(--text-muted);cursor:pointer;background:none;border:none;text-decoration:underline}

        @media(max-width:360px){.card-wrapper{width:56px;height:80px}.card-value{font-size:1.1rem}}
    </style>
</head>
<body>
<div class="toast-container" id="toastContainer"></div>

<!-- ========== LOGIN SECRETO DE ADMIN ========== -->
<div id="loginScreen" class="app-container hidden">
    <div class="login-screen">
        <div class="login-logo">🔐 Royal<span>High</span></div>
        <p style="font-size:.7rem;color:var(--text-muted);margin-bottom:1rem">Panel de Administración</p>
        <input type="email" id="loginEmail" class="login-input" placeholder="Correo electrónico">
        <input type="password" id="loginPassword" class="login-input" placeholder="Contraseña">
        <div class="login-error" id="loginError"></div>
        <button id="btnLogin" class="btn-login">Ingresar</button>
        <button id="btnForgotPassword" class="forgot-link">¿Olvidaste tu contraseña?</button>
        <p style="font-size:.55rem;color:var(--text-muted);margin-top:16px;border-top:1px solid var(--border-subtle);padding-top:12px">Acceso exclusivo para administradores</p>
    </div>
</div>

<!-- ========== VISTA DE JUEGO ========== -->
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
            <div class="progress-header">
                <span id="txt-tope-izquierda">Bono: $15,000</span>
                <span id="txt-tope-derecha">Tope: $35K</span>
            </div>
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

<!-- ========== PANEL DE ADMINISTRACIÓN ========== -->
<div id="adminView" class="app-container hidden">
    <div id="sectionAdmin" class="section active">
        <div class="unified-header">
            <div class="avatar" id="adminAvatar">A</div>
            <div style="flex:1;min-width:0">
                <div class="user-name" id="adminUsername">Admin</div>
                <div style="font-size:.6rem;color:var(--success)">Panel de Control</div>
            </div>
            <button id="btnAdminLogout" class="btn-sm btn-out">Salir</button>
        </div>
        <div class="admin-panel">
            <div class="admin-title">📊 Estadísticas</div>
            <div class="admin-stats">
                <div class="admin-stat"><div class="admin-stat-label">Online</div><div class="admin-stat-value" id="adminOnline">0</div></div>
                <div class="admin-stat"><div class="admin-stat-label">Jugando</div><div class="admin-stat-value" id="adminPlaying">0</div></div>
                <div class="admin-stat"><div class="admin-stat-label">Comisión</div><div class="admin-stat-value" id="adminComision">$0</div></div>
                <div class="admin-stat"><div class="admin-stat-label">Movido</div><div class="admin-stat-value" id="adminMovido">$0</div></div>
            </div>
        </div>
        <div class="admin-panel">
            <div class="admin-title">💰 Recargar Usuario</div>
            <div class="admin-recarga"><input type="email" id="adminUserEmail" placeholder="Email usuario" style="flex:2"><input type="number" id="adminMontoRecarga" placeholder="$Monto" min="1000" style="flex:1"><button id="btnAdminVerSaldo" class="btn-admin-ver">🔍</button><button id="btnAdminRecargar" class="btn-admin-recargar">+</button></div>
            <div class="admin-saldo-info" id="adminSaldoInfo"></div>
        </div>
        <div class="admin-panel">
            <div class="admin-title">🔑 Activar Cuenta (Invitado → Real)</div>
            <div class="admin-recarga"><input type="email" id="adminActivarEmail" placeholder="Email del invitado" style="flex:2"><button id="btnAdminActivarCuenta" class="btn-admin-recargar" style="background:var(--gold);color:#000">Activar</button></div>
            <div class="admin-saldo-info" id="adminActivarInfo"></div>
        </div>
        <div class="admin-panel">
            <div class="admin-title">⚙️ Acciones</div>
            <div style="display:flex;gap:4px;flex-wrap:wrap">
                <button class="btn-admin-action" id="btnResetComision">↺ Comisión</button>
                <button class="btn-admin-action" id="btnResetMovido">↺ Movido</button>
                <button class="btn-admin-action" id="btnResetAll">🔄 Contadores</button>
                <button class="btn-admin-action" id="btnCleanRanking">🧹 Ranking</button>
                <button class="btn-admin-action" id="btnResetAllSaldo" style="width:100%;margin-top:4px">💀 Resetear TODOS los saldos</button>
            </div>
        </div>
    </div>
</div>

<!-- MODALES -->
<div id="activacionModal" class="modal-overlay hidden">
    <div class="modal-content">
        <div style="font-size:2.5rem;margin-bottom:8px">🌟</div>
        <div style="font-weight:700;font-size:1rem;color:var(--gold);margin-bottom:8px">¡Límite de Invitado Alcanzado!</div>
        <p style="font-size:.7rem;color:var(--text-secondary);margin-bottom:12px" id="activacionMsg"></p>
        <div class="llave-number"><span class="llave-logo">LLAVE</span> @3219401352</div>
        <p style="font-size:.6rem;color:var(--text-muted);margin:8px 0">Activa tu cuenta con <strong>$10,000</strong> y tu saldo será <strong>$30,000</strong> reales</p>
        <button id="btnActivarWhatsApp" class="btn-wa">📱 Activar por WhatsApp</button>
        <button id="btnCerrarActivacion" class="btn-modal-close">Cerrar</button>
    </div>
</div>

<div id="retiroModal" class="modal-overlay hidden">
    <div class="modal-content">
        <div style="font-size:2rem;margin-bottom:8px" id="retiroIcon">💸</div>
        <div style="font-weight:700;margin-bottom:4px" id="retiroTitle">Retirar Ganancias</div>
        <p style="font-size:.65rem;color:var(--text-muted);margin-bottom:12px" id="retiroSubtitle"></p>
        <div id="retiroSteps"></div>
        <button id="btnRetiroAccion" class="btn-wa" style="margin-top:10px"></button>
        <button id="btnRetiroCerrar" class="btn-modal-close">Cerrar</button>
    </div>
</div>

<script type="module">
import{initializeApp}from"https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
import{getAuth,signInWithEmailAndPassword,createUserWithEmailAndPassword,onAuthStateChanged,signOut,sendPasswordResetEmail}from"https://www.gstatic.com/firebasejs/10.8.1/firebase-auth.js";
import{getDatabase,ref,onValue,set,push,update,increment,runTransaction,get}from"https://www.gstatic.com/firebasejs/10.8.1/firebase-database.js";

const firebaseConfig={apiKey:"AIzaSyDh8g0vyKbRfPPOZ1C0M0guBr0EJ77AyF8",authDomain:"carta-facil-a4e22.firebaseapp.com",databaseURL:"https://carta-facil-a4e22-default-rtdb.firebaseio.com",projectId:"carta-facil-a4e22",storageBucket:"carta-facil-a4e22.appspot.com",messagingSenderId:"530927580102",appId:"1:530927580102:web:865211e2b16e84869038c7"};
const app=initializeApp(firebaseConfig),auth=getAuth(app),db=getDatabase(app);
const CORREO_ADMIN="dimarviafara07@gmail.com",WHATSAPP="573219401352";
const CLAVE_MAESTRA="Royal2024"; // Acceso de emergencia

// ============================================================================
// CONSTANTES FINANCIERAS
// ============================================================================
const BONO_BIENVENIDA=15000,TOPE_INVITADO=35000,COSTO_ACTIVACION=10000,UMBRAL_MINIMO_RETIRO=50000,SALDO_INICIAL_REAL=30000;

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

// ============================================================================
// BOTS
// ============================================================================
const NOMBRES_BOTS=["Juancho_88","Andrea.G","ElJefe_01","Camilo_RM","Diana_M","Santi_92","LauraP","PipeMaster","Sofi_22","DonJuego"];
function generarBotParaMesa(){return{id:'bot_'+Math.random().toString(36).substr(2,9),nombre:NOMBRES_BOTS[Math.floor(Math.random()*NOMBRES_BOTS.length)],esInvitado:false,esBot:true,saldo:100000};}

// ============================================================================
// FUNCIONES DEL SISTEMA
// ============================================================================
function ejecutarPartida(jugador,gananciaNeta){if(jugador.esBot){jugador.saldo+=gananciaNeta;return{puedeContinuar:true};}jugador.saldo+=gananciaNeta;if(jugador.esInvitado&&jugador.saldo>=TOPE_INVITADO){jugador.saldo=TOPE_INVITADO;return{puedeContinuar:false};}return{puedeContinuar:true};}
function procesarActivacionCuenta(jugador){if(!jugador.esInvitado)return{error:"Ya está activa."};jugador.saldo+=COSTO_ACTIVACION;jugador.saldo-=BONO_BIENVENIDA;jugador.esInvitado=false;return{estado:"ACTIVACION_EXITOSA",saldoActual:jugador.saldo};}
function solicitarRetiro(jugador,monto){if(jugador.esInvitado)return{estado:"ERROR",mensaje:"Activa tu cuenta."};if(jugador.saldo<UMBRAL_MINIMO_RETIRO)return{estado:"ERROR",mensaje:`Mínimo $${UMBRAL_MINIMO_RETIRO.toLocaleString('es-CO')}.`};if(monto>jugador.saldo)return{estado:"ERROR",mensaje:"Fondos insuficientes."};jugador.saldo-=monto;return{estado:"EXITO",saldoRestante:jugador.saldo};}

// ============================================================================
// ACTUALIZACIÓN DE COMPONENTES VISUALES
// ============================================================================
function actualizarComponentesVisuales(usuario){
    const txtSaldoSuperior=document.getElementById('txt-saldo-bono');
    const txtProgresoIntermedio=document.getElementById('texto-progreso-intermedio');
    const barraProgreso=document.getElementById('barra-progreso-llenado');
    const txtTopeIzquierda=document.getElementById('txt-tope-izquierda');
    const txtTopeDerecha=document.getElementById('txt-tope-derecha');
    const btnRetirar=document.getElementById('btn-retirar');
    const saldoLabel=document.getElementById('saldoLabel');
    const userTypeLabel=document.getElementById('userTypeLabel');
    const ua=document.getElementById('userAvatar');
    const tu=document.getElementById('txtUsername');
    const bReg=document.getElementById('btnRegistrarse');
    const bRec=document.getElementById('btnRecargar');
    const bL=document.getElementById('btnLogout');

    txtSaldoSuperior.textContent='$'+usuario.saldo.toLocaleString('es-CO');

    if(usuario.esInvitado){
        saldoLabel.textContent='BONO';userTypeLabel.textContent='Modo práctica';ua.textContent='👤';tu.textContent='Invitado';
        bReg.style.display='block';bRec.style.display='none';bL.style.display='none';
        txtTopeIzquierda.textContent='Bono: $15K';txtTopeDerecha.textContent='Tope: $35K';
        txtProgresoIntermedio.textContent='$'+usuario.saldo.toLocaleString('es-CO')+' / $'+TOPE_INVITADO.toLocaleString('es-CO');
        let p=(usuario.saldo/TOPE_INVITADO)*100;barraProgreso.style.width=Math.min(p,100)+'%';barraProgreso.style.backgroundColor='var(--gold)';
        btnRetirar.style.opacity='0.4';btnRetirar.style.backgroundColor='#451a23';btnRetirar.style.color='var(--danger)';btnRetirar.style.cursor='not-allowed';btnRetirar.disabled=true;
    }else{
        saldoLabel.textContent='SALDO';userTypeLabel.textContent='Jugador Real ✅';ua.textContent='R';tu.textContent='Real';
        bReg.style.display='none';bRec.style.display='block';bL.style.display='block';
        txtTopeIzquierda.textContent='Inicio: $30K';txtTopeDerecha.textContent='Meta: $50K';
        txtProgresoIntermedio.textContent='$'+usuario.saldo.toLocaleString('es-CO')+' / $'+UMBRAL_MINIMO_RETIRO.toLocaleString('es-CO');
        let p=(usuario.saldo/UMBRAL_MINIMO_RETIRO)*100;barraProgreso.style.width=Math.min(p,100)+'%';barraProgreso.style.backgroundColor='#10b981';
        if(usuario.saldo>=UMBRAL_MINIMO_RETIRO){btnRetirar.style.opacity='1.0';btnRetirar.style.backgroundColor='#10b981';btnRetirar.style.color='#fff';btnRetirar.style.cursor='pointer';btnRetirar.disabled=false;}
        else{btnRetirar.style.opacity='0.5';btnRetirar.style.backgroundColor='#1e293b';btnRetirar.style.color='var(--text-muted)';btnRetirar.style.cursor='not-allowed';btnRetirar.disabled=true;}
    }
}

function mT(m,t='info'){const d=document.createElement('div');d.className=`toast ${t}`;d.textContent=m;document.getElementById('toastContainer').appendChild(d);setTimeout(()=>{d.style.opacity='0';d.style.transition='opacity .3s';setTimeout(()=>d.remove(),300);},3000);}

// ============================================================================
// DETECCIÓN DE RUTA SECRETA
// ============================================================================
const esRutaAdmin=window.location.hash==='#control-master';
const loginScreen=document.getElementById('loginScreen');
const gameView=document.getElementById('gameView');
const adminView=document.getElementById('adminView');
const loginError=document.getElementById('loginError');

let jugador={nombre:'Invitado',saldo:BONO_BIENVENIDA,esInvitado:true,esBot:false};

// ========== FLUJO DE ADMIN ==========
if(esRutaAdmin){
    loginScreen.classList.remove('hidden');
    
    // ✅ RECUPERAR CONTRASEÑA
    document.getElementById('btnForgotPassword').addEventListener('click',async()=>{
        const email=document.getElementById('loginEmail').value.trim();
        if(!email){loginError.textContent='❌ Ingresa tu correo para recuperar contraseña';return;}
        try{
            await sendPasswordResetEmail(auth,email);
            loginError.textContent='✅ Correo de recuperación enviado. Revisa tu bandeja.';
            loginError.style.color='var(--success)';
        }catch(e){
            loginError.textContent='❌ Error al enviar. Verifica el correo.';
            loginError.style.color='var(--danger)';
        }
    });
    
    // ✅ LOGIN
    document.getElementById('btnLogin').addEventListener('click',async()=>{
        const email=document.getElementById('loginEmail').value.trim();
        const password=document.getElementById('loginPassword').value;
        
        if(!email||!password){loginError.textContent='Completa todos los campos';loginError.style.color='var(--danger)';return;}
        
        // ⚠️ ACCESO DE EMERGENCIA CON CLAVE MAESTRA
        if(email===CORREO_ADMIN&&password===CLAVE_MAESTRA){
            loginScreen.classList.add('hidden');adminView.classList.remove('hidden');
            document.getElementById('adminUsername').textContent='Admin (Emergencia)';
            document.getElementById('adminAvatar').textContent='A';
            initAdminPanel('emergencia');sf();mT('✅ Acceso de emergencia concedido','success');return;
        }
        
        document.getElementById('btnLogin').disabled=true;
        document.getElementById('btnLogin').textContent='Verificando...';
        loginError.textContent='';
        
        try{
            const cred=await signInWithEmailAndPassword(auth,email,password);
            const userSnap=await get(ref(db,`usuarios/${cred.user.uid}`));
            const userData=userSnap.val()||{};
            
            if(userData.rol==='admin'){
                loginScreen.classList.add('hidden');adminView.classList.remove('hidden');
                document.getElementById('adminUsername').textContent=userData.alias||'Admin';
                document.getElementById('adminAvatar').textContent=(userData.alias||'A').charAt(0).toUpperCase();
                initAdminPanel(cred.user.uid);sf();mT('✅ Acceso de administrador concedido','success');
            }else{
                loginError.textContent='⛔ Acceso denegado. No tienes permisos de administrador.';
                loginError.style.color='var(--danger)';
                await signOut(auth);
            }
        }catch(e){
            if(e.code==='auth/user-not-found'||e.code==='auth/wrong-password'){
                loginError.textContent='❌ Correo o contraseña incorrectos';
            }else if(e.code==='auth/invalid-email'){
                loginError.textContent='❌ Correo electrónico no válido';
            }else if(e.code==='auth/too-many-requests'){
                loginError.textContent='❌ Demasiados intentos. Espera unos minutos.';
            }else{
                loginError.textContent='❌ Error: '+e.message;
            }
            loginError.style.color='var(--danger)';
        }
        document.getElementById('btnLogin').disabled=false;
        document.getElementById('btnLogin').textContent='Ingresar';
    });
    
    document.getElementById('loginPassword').addEventListener('keypress',e=>{if(e.key==='Enter')document.getElementById('btnLogin').click();});
    
}else{
    // ========== FLUJO NORMAL DE JUGADOR ==========
    loginScreen.classList.add('hidden');adminView.classList.add('hidden');gameView.classList.remove('hidden');
    initGameUI();
}

// ========== PANEL DE ADMINISTRACIÓN ==========
function initAdminPanel(adminUid){
    onValue(ref(db,'adminData'),snap=>{const d=snap.val()||{};document.getElementById('adminComision').textContent='$'+(d.total_comisiones||0).toLocaleString('es-CO');document.getElementById('adminMovido').textContent='$'+(d.dinero_movido||0).toLocaleString('es-CO');document.getElementById('adminPlaying').textContent=d.partidas_jugando||0;});
    onValue(ref(db,'presencia'),snap=>{const p=snap.val()||{};document.getElementById('adminOnline').textContent=Object.values(p).filter(v=>v.status==='online').length;});
    document.getElementById('btnAdminVerSaldo').addEventListener('click',async()=>{
        const eb=document.getElementById('adminUserEmail').value.trim().toLowerCase();const si=document.getElementById('adminSaldoInfo');
        if(!eb){si.textContent='❌ Ingresa un email';return;}si.textContent='🔍 Buscando...';
        try{const usS=await get(ref(db,'usuarios'));const us=usS.val()||{};let en=null;for(const uid in us){if(us[uid].email&&us[uid].email.toLowerCase()===eb){en={uid,alias:us[uid].alias||'Sin alias',saldo:us[uid].saldo||0};break;}}if(en){si.innerHTML=`✅ <strong>${en.alias}</strong> - Saldo: <strong style="color:var(--gold)">$${en.saldo.toLocaleString('es-CO')}</strong>`;}else{si.textContent='❌ Usuario no encontrado';}}catch(e){si.textContent='❌ Error al buscar';}
    });
    document.getElementById('btnAdminRecargar').addEventListener('click',async()=>{
        const eb=document.getElementById('adminUserEmail').value.trim().toLowerCase(),mo=parseInt(document.getElementById('adminMontoRecarga').value),si=document.getElementById('adminSaldoInfo');
        if(!eb||!mo||mo<1000)return mT('❌ Email y monto mínimo $1.000','error');
        try{const usS=await get(ref(db,'usuarios')),us=usS.val()||{};let uidE=null,alE='';for(const uid in us){if(us[uid].email&&us[uid].email.toLowerCase()===eb){uidE=uid;alE=us[uid].alias||'Usuario';break;}}if(!uidE)return mT('❌ Usuario no encontrado','error');const sRef=ref(db,`usuarios/${uidE}/saldo`);await runTransaction(sRef,(s)=>(s||0)+mo);const ns=(await get(sRef)).val();si.innerHTML=`✅ <strong>${alE}</strong> - Nuevo saldo: <strong style="color:var(--gold)">$${ns.toLocaleString('es-CO')}</strong>`;sf();mT(`✅ $${mo.toLocaleString('es-CO')} a ${alE}`,'success');document.getElementById('adminMontoRecarga').value='';}catch(e){mT('❌ Error','error');}
    });
    document.getElementById('btnAdminActivarCuenta').addEventListener('click',async()=>{
        const eb=document.getElementById('adminActivarEmail').value.trim().toLowerCase();const ai=document.getElementById('adminActivarInfo');
        if(!eb){ai.textContent='❌ Ingresa un email';return;}
        try{const usS=await get(ref(db,'usuarios'));const us=usS.val()||{};let uidE=null,alE='',saE=0;for(const uid in us){if(us[uid].email&&us[uid].email.toLowerCase()===eb){uidE=uid;alE=us[uid].alias||'Usuario';saE=us[uid].saldo||0;break;}}if(!uidE){ai.textContent='❌ Usuario no encontrado';return;}const ns=Math.min(saE,TOPE_INVITADO)+COSTO_ACTIVACION-BONO_BIENVENIDA;await update(ref(db,`usuarios/${uidE}`),{saldo:ns,esInvitado:false,rol:'jugador'});ai.innerHTML=`✅ <strong>${alE}</strong> activado. Saldo: <strong style="color:var(--gold)">$${ns.toLocaleString('es-CO')}</strong>`;sf();mT(`✅ ${alE} activado`,'success');document.getElementById('adminActivarEmail').value='';}catch(e){ai.textContent='❌ Error';}
    });
    document.getElementById('btnResetComision').addEventListener('click',()=>update(ref(db,'adminData'),{total_comisiones:0}));
    document.getElementById('btnResetMovido').addEventListener('click',()=>update(ref(db,'adminData'),{dinero_movido:0}));
    document.getElementById('btnResetAll').addEventListener('click',async()=>{await set(ref(db,'adminData'),{total_comisiones:0,dinero_movido:0,partidas_jugando:0});mT('✅ Contadores reiniciados','success');});
    document.getElementById('btnCleanRanking').addEventListener('click',async()=>{const snap=await get(ref(db,'usuarios'));const us=snap.val()||{};for(const uid in us){if(us[uid].historial)await set(ref(db,`usuarios/${uid}/historial`),null);}mT('✅ Ranking limpiado','success');});
    document.getElementById('btnResetAllSaldo').addEventListener('click',async()=>{if(!confirm('⚠️ ¿Resetear TODOS los saldos a $0?'))return;const snap=await get(ref(db,'usuarios'));const us=snap.val()||{};for(const uid in us){if(us[uid].saldo&&us[uid].saldo>0)await set(ref(db,`usuarios/${uid}/saldo`),0);}mT('✅ Saldos reseteados','success');});
    document.getElementById('btnAdminLogout').addEventListener('click',async()=>{await signOut(auth);adminView.classList.add('hidden');loginScreen.classList.remove('hidden');document.getElementById('loginEmail').value='';document.getElementById('loginPassword').value='';loginError.textContent='';});
}

// ========== INICIALIZAR UI DE JUEGO ==========
function initGameUI(){
    const tC=document.getElementById('tableContent'),bP=document.getElementById('btnPlay'),pA=document.getElementById('potAmount');
    const sI=document.getElementById('searchingIndicator'),m1=document.getElementById('mode1v1'),m3=document.getElementById('mode3p');
    const bCs=document.querySelectorAll('.bet-chip');
    const activacionModal=document.getElementById('activacionModal'),activacionMsg=document.getElementById('activacionMsg');
    const btnActivarWhatsApp=document.getElementById('btnActivarWhatsApp'),btnCerrarActivacion=document.getElementById('btnCerrarActivacion');
    const retiroModal=document.getElementById('retiroModal'),retiroIcon=document.getElementById('retiroIcon');
    const retiroTitle=document.getElementById('retiroTitle'),retiroSubtitle=document.getElementById('retiroSubtitle');
    const retiroSteps=document.getElementById('retiroSteps'),btnRetiroAccion=document.getElementById('btnRetiroAccion');
    const btnRetiroCerrar=document.getElementById('btnRetiroCerrar'),sd=document.getElementById('txt-saldo-bono');
    
    let apAct=2000,mJ='1v1',eP=false;
    const pintas=[{icon:'♠',color:'#1E293B'},{icon:'♥',color:'#DC2626'},{icon:'♦',color:'#2563EB'},{icon:'♣',color:'#16A34A'}];
    const nC=["2","3","4","5","6","7","8","9","10","J","Q","K","A"],MJ={'1v1':2,'3p':3};
    const esp=(ms)=>new Promise(r=>setTimeout(r,ms));
    let mazoVirtual=[];
    function crearMazo(){const m=[];for(let vi=0;vi<nC.length;vi++){for(let pi=0;pi<pintas.length;pi++){m.push({peso:vi,texto:nC[vi],pinta:pintas[pi]});}}return m;}
    function barajarMazo(mazo){const mc=[...mazo];for(let i=mc.length-1;i>0;i--){const a=new Uint32Array(1);crypto.getRandomValues(a);const j=Math.floor((a[0]/0xFFFFFFFF)*(i+1));[mc[i],mc[j]]=[mc[j],mc[i]];}return mc;}
    function repartirCartas(c){if(mazoVirtual.length<c)mazoVirtual=barajarMazo(crearMazo());return mazoVirtual.splice(0,c);}
    function resetMazo(){mazoVirtual=barajarMazo(crearMazo());}
    function updPot(c){if(pA)pA.textContent='$'+(apAct*c).toLocaleString('es-CO')}
    function updTbl(){if(eP)return;const m=MJ[mJ];let h='<div class="cards-row">';for(let i=0;i<m;i++)h+=`<div class="card-slot"><div class="card-wrapper"><div class="card-inner"><div class="card-face card-back"><div style="font-size:1.2rem;font-weight:700;color:rgba(201,168,76,0.4)">♠♥</div></div><div class="card-face card-front"><div class="card-value" style="color:var(--text-muted)">?</div></div></div></div><div class="card-label">${i===0?'Tú':'Jugador '+(i+1)}</div></div>`;tC.innerHTML=h+'</div>';}
    updPot(2);updTbl();
    
    m1.addEventListener('click',()=>{if(eP)return;sc();m1.classList.add('selected');m3.classList.remove('selected');mJ='1v1';updPot(2);updTbl();});
    m3.addEventListener('click',()=>{if(eP)return;sc();m3.classList.add('selected');m1.classList.remove('selected');mJ='3p';updPot(3);updTbl();});
    bCs.forEach(ch=>{ch.addEventListener('click',()=>{if(eP)return;sc();bCs.forEach(c=>c.classList.remove('selected'));ch.classList.add('selected');apAct=parseInt(ch.dataset.apuesta);updPot(MJ[mJ]);})});
    
    async function animarReparto(jugadores){
        tC.innerHTML='<div class="cards-row"></div>';const row=tC.querySelector('.cards-row');
        for(let i=0;i<jugadores.length;i++){const p=jugadores[i];const et=!p.esBot&&p.nombre===jugador.nombre;const slot=document.createElement('div');slot.className='card-slot'+(et?' tu-carta':'');slot.innerHTML=`<div class="card-wrapper" id="cw${i}"><div class="card-inner"><div class="card-face card-back"><div style="font-size:1.2rem;font-weight:700;color:rgba(201,168,76,0.4)">♠♥</div></div><div class="card-face card-front"><div class="card-value" style="color:${p.carta.pinta.color}">${p.carta.texto}</div><div style="font-size:1rem;color:${p.carta.pinta.color}">${p.carta.pinta.icon}</div></div></div></div><div class="card-label ${et?'tu-label':''}">${et?'Tú':p.nombre}</div>`;row.appendChild(slot);sDeal();await esp(400);}
    }
    async function animarVolteo(){const w=document.querySelectorAll('.card-wrapper');for(let i=0;i<w.length;i++){await esp(400);w[i].classList.add('flipped');sFlip();}}
    
    function mostrarModalActivacion(){activacionMsg.textContent=`Has alcanzado $${TOPE_INVITADO.toLocaleString('es-CO')}. Activa tu cuenta con $${COSTO_ACTIVACION.toLocaleString('es-CO')} y obtén $${SALDO_INICIAL_REAL.toLocaleString('es-CO')} reales.`;activacionModal.classList.remove('hidden');sf();}
    
    // ✅ BOTÓN JUGAR
    bP.addEventListener('click',async()=>{
        if(eP){eP=false;bP.disabled=false;bP.textContent='Buscar Partida';bP.classList.remove('cooldown');sI.classList.add('hidden');updTbl();sc();return;}
        if(jugador.saldo<apAct)return mT('❌ Saldo insuficiente','error');
        if(jugador.esInvitado&&jugador.saldo>=TOPE_INVITADO){mostrarModalActivacion();return;}
        sc();eP=true;bP.disabled=true;bP.textContent='Buscando...';bP.classList.add('canceling');sI.classList.remove('hidden');sb();
        await esp(1000+Math.random()*1500);if(!eP)return;
        resetMazo();const maxJ=MJ[mJ];const cartas=repartirCartas(maxJ);const botsNecesarios=maxJ-1;
        const jugadores=[{...jugador,carta:cartas[0]}];
        for(let i=0;i<botsNecesarios;i++){const bot=generarBotParaMesa();bot.carta=cartas[i+1];jugadores.push(bot);}
        sI.classList.add('hidden');bP.textContent='Repartiendo...';bP.classList.remove('canceling');bP.classList.add('cooldown');scart();
        await animarReparto(jugadores);await esp(800);bP.textContent='Revelando...';await animarVolteo();await esp(800);
        const pesos=jugadores.map(j=>j.carta.peso);const maxP=Math.max(...pesos);let ganadores=jugadores.filter(j=>j.carta.peso===maxP);let ganador=ganadores[0];
        if(ganadores.length>1){const pp={'♠':4,'♥':3,'♦':2,'♣':1};ganador=ganadores.reduce((a,b)=>(pp[b.carta.pinta.icon]||0)>(pp[a.carta.pinta.icon]||0)?b:a);}
        const yoGane=!ganador.esBot&&ganador.nombre===jugador.nombre;const tPot=apAct*maxJ;
        if(yoGane){const ganancia=tPot-apAct;const resultado=ejecutarPartida(jugador,ganancia);sg();sd.classList.add('up');setTimeout(()=>sd.classList.remove('up'),600);mT(`🎉 ¡Ganaste $${ganancia.toLocaleString('es-CO')}!`,'success');bP.textContent='🎉 ¡Ganaste!';actualizarComponentesVisuales(jugador);if(!resultado.puedeContinuar){await esp(3000);mostrarModalActivacion();resetSearch();return;}}
        else{ejecutarPartida(jugador,-apAct);sp();sd.classList.add('down');setTimeout(()=>sd.classList.remove('down'),600);mT(`😔 Perdiste $${apAct.toLocaleString('es-CO')}. ${ganador.nombre} ganó con ${ganador.carta.texto}${ganador.carta.pinta.icon}`,'warning');bP.textContent='😔 Perdiste';actualizarComponentesVisuales(jugador);}
        document.querySelectorAll('.card-label').forEach((label,i)=>{if(jugadores[i]?.nombre===ganador.nombre)label.classList.add('winner');else if(!jugadores[i]?.esBot&&!yoGane)label.classList.add('loser');});
        await esp(4000);resetSearch();
    });
    
    function resetSearch(){eP=false;bP.disabled=false;bP.textContent='Buscar Partida';bP.classList.remove('canceling','cooldown');sI.classList.add('hidden');updPot(MJ[mJ]);updTbl();}
    
    btnActivarWhatsApp.addEventListener('click',()=>{sc();activacionModal.classList.add('hidden');window.open(`https://wa.me/${WHATSAPP}?text=${encodeURIComponent(`Hola, quiero activar mi cuenta. Tengo $${jugador.saldo.toLocaleString('es-CO')} y quiero hacer la recarga de $${COSTO_ACTIVACION.toLocaleString('es-CO')}.`)}`,'_blank');mT('📱 Envía el comprobante.','info');});
    btnCerrarActivacion.addEventListener('click',()=>{sc();activacionModal.classList.add('hidden');if(jugador.saldo>=TOPE_INVITADO){mT('⚠️ No puedes seguir jugando hasta activar.','warning');bP.disabled=true;bP.textContent='Activa tu cuenta';}});
    
    // ✅ BOTÓN REGISTRARSE - MANDA DIRECTAMENTE AL LOBBY
    document.getElementById('btnRegistrarse').addEventListener('click',()=>{
        sc();
        if(jugador.saldo>=TOPE_INVITADO){
            mostrarModalActivacion();
        }else{
            // Abrir WhatsApp para registro
            window.open(`https://wa.me/${WHATSAPP}?text=${encodeURIComponent(`Hola, quiero registrarme en Royal High.`)}`,'_blank');
            mT('📝 Regístrate por WhatsApp y compite por dinero real.','info');
        }
    });
    
    document.getElementById('btn-retirar').addEventListener('click',()=>{
        sc();const resultado=solicitarRetiro(jugador,jugador.saldo);
        if(resultado.estado==="ERROR"){
            if(jugador.esInvitado){retiroIcon.textContent='🔒';retiroTitle.textContent='Cuenta Invitado';retiroSubtitle.textContent='Debes activar tu cuenta para retirar';retiroSteps.innerHTML=`<div class="retiro-step bloqueado">❌ Cuenta invitado</div><div class="retiro-step pendiente">📱 Activa con $${COSTO_ACTIVACION.toLocaleString('es-CO')}</div><div class="retiro-step activo">💰 Saldo real: $${SALDO_INICIAL_REAL.toLocaleString('es-CO')}</div>`;btnRetiroAccion.textContent='📱 Activar por WhatsApp';btnRetiroAccion.onclick=()=>{retiroModal.classList.add('hidden');mostrarModalActivacion();};}
            else{const faltante=UMBRAL_MINIMO_RETIRO-jugador.saldo;retiroIcon.textContent='🎯';retiroTitle.textContent='Meta de Retiro';retiroSubtitle.textContent=`Te faltan $${faltante.toLocaleString('es-CO')}`;retiroSteps.innerHTML=`<div class="retiro-step activo">✅ $${jugador.saldo.toLocaleString('es-CO')}</div><div class="retiro-step pendiente">🎯 $${UMBRAL_MINIMO_RETIRO.toLocaleString('es-CO')}</div><div class="retiro-step bloqueado">⏳ Faltan $${faltante.toLocaleString('es-CO')}</div>`;btnRetiroAccion.textContent='Seguir jugando';btnRetiroAccion.onclick=()=>{retiroModal.classList.add('hidden');};}
        }else{sf();retiroIcon.textContent='✅';retiroTitle.textContent='¡Retiro Aprobado!';retiroSubtitle.textContent=resultado.mensaje;retiroSteps.innerHTML=`<div class="retiro-step activo">✅ $${jugador.saldo.toLocaleString('es-CO')}</div><div class="retiro-step activo">✅ Meta alcanzada</div>`;btnRetiroAccion.textContent='📱 Solicitar por WhatsApp';btnRetiroAccion.onclick=()=>{retiroModal.classList.add('hidden');const llave=prompt('📱 Llave Bre-B:');if(!llave)return;window.open(`https://wa.me/${WHATSAPP}?text=${encodeURIComponent(`Retiro de $${jugador.saldo.toLocaleString('es-CO')}. Llave: ${llave}`)}`,'_blank');actualizarComponentesVisuales(jugador);};}
        retiroModal.classList.remove('hidden');
    });
    btnRetiroCerrar.addEventListener('click',()=>{sc();retiroModal.classList.add('hidden');});
    document.getElementById('btnRecargar').addEventListener('click',()=>{sc();if(jugador.esInvitado){mT('📝 Activa tu cuenta primero.','info');return;}const monto=prompt('Monto a recargar (mínimo $10,000):');if(!monto||parseInt(monto)<10000)return mT('❌ Mínimo $10,000','error');window.open(`https://wa.me/${WHATSAPP}?text=${encodeURIComponent(`Recarga de $${parseInt(monto).toLocaleString('es-CO')} en Royal High.`)}`,'_blank');mT('📱 Envía tu comprobante.','info');});
    document.getElementById('btnLogout').addEventListener('click',()=>{signOut(auth);location.reload();});
    
    // ✅ Auth state
    onAuthStateChanged(auth,async(user)=>{
        if(user){
            const uSnap=await get(ref(db,`usuarios/${user.uid}`));const uData=uSnap.val()||{};
            if(uData.rol==='admin')return;
            jugador.nombre=uData.alias||user.email.split('@')[0];
            jugador.saldo=uData.saldo||0;
            jugador.esInvitado=uData.esInvitado!==false;
            actualizarComponentesVisuales(jugador);
        }
    });
    
    actualizarComponentesVisuales(jugador);
    sm();
    setTimeout(()=>mT('🎮 ¡Bienvenido! Elige modalidad y presiona Buscar Partida.','info'),1000);
}
</script>
</body>
</html>
