import { useState, useRef, useCallback, useEffect } from 'react';
import { Copy, Check, RotateCcw } from 'lucide-react';
import { supabase } from '@/lib/supabase';

interface SpinResult {
  spinNumber: number;
  spinsRemaining: number;
  wonPrize: boolean;
  promoCode: { code: string; label: string } | null;
}

const SEGMENTS = [
  { label: 'TRY AGAIN', color: '#180928', glow: '#4ade80', isWin: false },
  { label: 'WIN!', color: '#ef4444', glow: '#ffffff', isWin: true },
  { label: 'MAYBE NEXT', color: '#052e16', glow: '#22c55e', isWin: false },
  { label: 'WIN!', color: '#a855f7', glow: '#ffffff', isWin: true },
  { label: 'TRY AGAIN', color: '#7f1d1d', glow: '#fca5a5', isWin: false },
  { label: 'WIN!', color: '#22c55e', glow: '#022c22', isWin: true },
  { label: 'MAYBE NEXT', color: '#0f051a', glow: '#4ade80', isWin: false },
  { label: 'WIN!', color: '#eab308', glow: '#022c22', isWin: true },
] as const;

const SEGMENT_COUNT = SEGMENTS.length;
const SEGMENT_ANGLE = 360 / SEGMENT_COUNT;

const FLOATING_SPOOKY = [
  { emoji: '\uD83E\uDD87', className: 'bat-1' },
  { emoji: '\uD83E\uDD87', className: 'bat-2' },
  { emoji: '\uD83D\uDC7B', className: 'ghost-1' },
  { emoji: '\uD83D\uDC7B', className: 'ghost-2' },
  { emoji: '\uD83C\uDF83', className: 'jack-1' },
  { emoji: '\uD83C\uDF83', className: 'jack-2' },
  { emoji: '\uD83D\uDC80', className: 'skel-1' },
  { emoji: '\uD83D\uDE08', className: 'smile-bg' },
];

const WEB_CORNERS = [
  { className: 'web-tl', flip: '' },
  { className: 'web-tr', flip: 'scaleX(-1)' },
  { className: 'web-bl', flip: 'scaleY(-1)' },
  { className: 'web-br', flip: 'scale(-1)' },
];

function getSessionId(): string {
  const KEY = 'wof_session_id';
  let id = localStorage.getItem(KEY);
  if (!id) {
    id = crypto.randomUUID();
    localStorage.setItem(KEY, id);
  }
  return id;
}

export default function App() {
  const [spinsRemaining, setSpinsRemaining] = useState(3);
  const [isSpinning, setIsSpinning] = useState(false);
  const [hasWon, setHasWon] = useState(false);
  const [promoCode, setPromoCode] = useState<{ code: string; label: string } | null>(null);
  const [copied, setCopied] = useState(false);
  const [result, setResult] = useState<SpinResult | null>(null);
  const [showResultModal, setShowResultModal] = useState(false);
  const [spinNumber, setSpinNumber] = useState(0);
  const [errorMsg, setErrorMsg] = useState<string | null>(null);

  const rotationRef = useRef(0);
  const wheelRef = useRef<HTMLDivElement>(null);
  const sessionIdRef = useRef<string | null>(null);

  useEffect(() => {
    sessionIdRef.current = getSessionId();
  }, []);

  const getOrCreateSession = useCallback(async () => {
    const sessionId = sessionIdRef.current!;
    const { data: existing } = await supabase
      .from('game_sessions')
      .select('id, spins_used, has_won, promo_code_id')
      .eq('id', sessionId)
      .maybeSingle();

    if (existing) {
      setSpinsRemaining(3 - existing.spins_used);
      setSpinNumber(existing.spins_used);
      setHasWon(existing.has_won);
    } else {
      const { error } = await supabase
        .from('game_sessions')
        .insert({ id: sessionId, spins_used: 0, has_won: false });
      if (error) {
        console.error('Session create error:', error);
      }
    }
  }, []);

  useEffect(() => {
    getOrCreateSession();
  }, [getOrCreateSession]);

  const handleSpin = useCallback(async () => {
    if (isSpinning || spinsRemaining <= 0) return;

    setErrorMsg(null);
    setIsSpinning(true);
    setShowResultModal(false);

    const currentSpin = spinNumber + 1;
    const shouldWin = currentSpin === 3;

    const winSegments = SEGMENTS.filter((s) => s.isWin);
    const loseSegments = SEGMENTS.filter((s) => !s.isWin);
    const targetPool = shouldWin ? winSegments : loseSegments;
    const targetIndex = SEGMENTS.indexOf(targetPool[Math.floor(Math.random() * targetPool.length)]);

    const segmentCenter = targetIndex * SEGMENT_ANGLE + SEGMENT_ANGLE / 2;
    const extraSpins = 5 + Math.floor(Math.random() * 3);
    const offset = (Math.random() - 0.5) * (SEGMENT_ANGLE * 0.6);

    const targetRotation = rotationRef.current + extraSpins * 360 + (360 - segmentCenter) + offset;
    rotationRef.current = targetRotation;

    if (wheelRef.current) {
      wheelRef.current.style.transition = 'transform 5s cubic-bezier(0.17, 0.67, 0.12, 0.99)';
      wheelRef.current.style.transform = `rotate(${targetRotation}deg)`;
    }

    setTimeout(async () => {
      try {
        const response = await fetch(
          `${import.meta.env.VITE_SUPABASE_URL}/functions/v1/spin`,
          {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
              Authorization: `Bearer ${import.meta.env.VITE_SUPABASE_ANON_KEY}`,
            },
            body: JSON.stringify({ sessionId: sessionIdRef.current }),
          },
        );

        if (!response.ok) {
          throw new Error(`Request failed (${response.status})`);
        }

        const data: SpinResult = await response.json();

        if (data.error && data.spinsRemaining === 0) {
          setSpinsRemaining(0);
          setIsSpinning(false);
          return;
        }

        setResult(data);
        setSpinsRemaining(data.spinsRemaining);
        setSpinNumber(data.spinNumber);
        setHasWon(data.wonPrize);

        if (data.wonPrize && data.promoCode) {
          setPromoCode(data.promoCode);
        }

        setIsSpinning(false);
        setShowResultModal(true);
      } catch (err) {
        console.error('Spin error:', err);
        setErrorMsg('Something went wrong. Please try again.');
        setIsSpinning(false);
      }
    }, 5200);
  }, [isSpinning, spinsRemaining, spinNumber]);

  const handleCopy = useCallback(() => {
    if (promoCode) {
      navigator.clipboard.writeText(promoCode.code);
      setCopied(true);
      setTimeout(() => setCopied(false), 2000);
    }
  }, [promoCode]);

  const handleReset = useCallback(async () => {
    const newId = crypto.randomUUID();
    localStorage.setItem('wof_session_id', newId);
    sessionIdRef.current = newId;

    setSpinsRemaining(3);
    setSpinNumber(0);
    setHasWon(false);
    setPromoCode(null);
    setResult(null);
    setShowResultModal(false);
    setCopied(false);
    rotationRef.current = 0;

    if (wheelRef.current) {
      wheelRef.current.style.transition = 'none';
      wheelRef.current.style.transform = 'rotate(0deg)';
    }

    const { error } = await supabase
      .from('game_sessions')
      .insert({ id: newId, spins_used: 0, has_won: false });
    if (error) console.error('Session create error:', error);
  }, []);

  return (
    <div className="spooky-app min-h-screen relative overflow-x-hidden overflow-y-auto flex flex-col items-center justify-between px-3 py-4 gap-3">
      {/* Web corners */}
      {WEB_CORNERS.map((w) => (
        <div
          key={w.className}
          className="absolute pointer-events-none z-1 select-none"
          style={{
            fontSize: 'clamp(2.5rem, 4vw, 4rem)',
            opacity: 0.35,
            lineHeight: 1,
            ...(w.className === 'web-tl' && { top: '6px', left: '6px' }),
            ...(w.className === 'web-tr' && { top: '6px', right: '6px', transform: w.flip }),
            ...(w.className === 'web-bl' && { bottom: '6px', left: '6px', transform: w.flip }),
            ...(w.className === 'web-br' && { bottom: '6px', right: '6px', transform: w.flip }),
          }}
        >
          {'\uD83D\uDD78\uFE0F'}
        </div>
      ))}

      {/* Floating spooky emojis */}
      {FLOATING_SPOOKY.map((s, i) => (
        <div
          key={i}
          className={`spooky-float absolute pointer-events-none z-1 select-none ${s.className}`}
        >
          {s.emoji}
        </div>
      ))}

      {/* Content */}
      <header className="text-center z-10 relative">
        <h1
          className="spooky-title bloody-text"
          style={{ fontSize: 'clamp(1.8rem, 6vw, 3.6rem)' }}
        >
          TRICK OR TREAT {'\uD83C\uDF83'}{'\uDE78'}
        </h1>
        <p className="spooky-subtitle">
          Spin the cursed wheel 3 times — the third spin always reveals a promo code!
        </p>
      </header>

      {/* Attempts badge */}
      <div className="attempts-badge z-10">
        {spinsRemaining} / 3 spins left {'\uD83D\uDD2E'}
      </div>

      {/* Wheel */}
      <div
        className="wheel-container relative z-10 flex justify-center items-center"
        style={{ width: 'min(80vw, 42vh, 340px)', height: 'min(80vw, 42vh, 340px)' }}
      >
        {/* Eye-ball pointer */}
        <div className="pointer-box absolute left-1/2 -top-6 z-20 flex flex-col items-center" style={{ transform: 'translateX(-50%)' }}>
          <div className="eye-ball">
            <div className="eye-pupil" />
          </div>
          <div
            style={{
              width: 0,
              height: 0,
              borderLeft: '14px solid transparent',
              borderRight: '14px solid transparent',
              borderTop: '26px solid #ef4444',
              filter: 'drop-shadow(0 2px 6px rgba(0,0,0,0.8))',
              marginTop: '-4px',
            }}
          />
        </div>

        {/* Wheel outer ring */}
        <div
          className="absolute inset-0 rounded-full"
          style={{
            border: '5px solid #05020a',
            boxShadow: '0 0 25px rgba(239,68,68,0.5), 0 0 18px rgba(34,197,94,0.4), inset 0 0 15px rgba(0,0,0,0.9)',
            background: 'radial-gradient(circle, #1a1a2e 0%, #05020a 100%)',
          }}
        />

        {/* Wheel segments */}
        <div
          ref={wheelRef}
          className="absolute rounded-full overflow-hidden"
          style={{ inset: '8px', willChange: 'transform' }}
        >
          {SEGMENTS.map((seg, i) => {
            const startAngle = i * SEGMENT_ANGLE;
            const endAngle = startAngle + SEGMENT_ANGLE;
            const midAngle = startAngle + SEGMENT_ANGLE / 2;

            const cx = 50;
            const cy = 50;
            const r = 50;
            const startX = cx + r * Math.cos((startAngle - 90) * Math.PI / 180);
            const startY = cy + r * Math.sin((startAngle - 90) * Math.PI / 180);
            const endX = cx + r * Math.cos((endAngle - 90) * Math.PI / 180);
            const endY = cy + r * Math.sin((endAngle - 90) * Math.PI / 180);

            const largeArc = SEGMENT_ANGLE > 180 ? 1 : 0;
            const path = `M ${cx} ${cy} L ${startX} ${startY} A ${r} ${r} 0 ${largeArc} 1 ${endX} ${endY} Z`;

            const labelRadius = 32;
            const labelX = cx + labelRadius * Math.cos((midAngle - 90) * Math.PI / 180);
            const labelY = cy + labelRadius * Math.sin((midAngle - 90) * Math.PI / 180);

            return (
              <svg
                key={i}
                className="absolute inset-0 w-full h-full"
                viewBox="0 0 100 100"
                preserveAspectRatio="none"
              >
                <path
                  d={path}
                  fill={seg.color}
                  stroke="#05020a"
                  strokeWidth="0.4"
                  opacity="0.92"
                />
                <text
                  x={labelX}
                  y={labelY}
                  fill={seg.glow}
                  fontSize="4.5"
                  fontWeight="bold"
                  textAnchor="middle"
                  dominantBaseline="middle"
                  transform={`rotate(${midAngle} ${labelX} ${labelY})`}
                  style={{ textShadow: '0 0 4px currentColor' }}
                >
                  {seg.label}
                </text>
              </svg>
            );
          })}
        </div>

        {/* Center cap */}
        <div
          className="absolute left-1/2 top-1/2 z-10 rounded-full flex items-center justify-center"
          style={{
            width: '44px',
            height: '44px',
            transform: 'translate(-50%, -50%)',
            background: '#090312',
            border: '3px solid #22c55e',
            boxShadow: '0 0 12px rgba(34,197,94,0.8)',
            fontSize: '1.2rem',
          }}
        >
          {'\uD83D\uDC80'}
        </div>
      </div>

      {/* Spin button */}
      <button
        onClick={handleSpin}
        disabled={isSpinning || spinsRemaining <= 0}
        className={`spooky-spin-btn z-10 ${isSpinning || spinsRemaining <= 0 ? 'spooky-spin-btn-disabled' : ''}`}
      >
        {isSpinning ? 'Spinning...' : spinsRemaining <= 0 ? 'Spent all attempts ' + '\uD83D\uDD12' : 'SPIN THE WHEEL ' + '\u26A1'}
      </button>

      {/* Reset button */}
      {spinsRemaining <= 0 && !isSpinning && (
        <button
          onClick={handleReset}
          className="z-10 flex items-center gap-2 px-6 py-2 text-sm transition-colors cursor-pointer"
          style={{ color: '#a8a8b3' }}
        >
          <RotateCcw className="w-4 h-4" />
          Play again
        </button>
      )}

      {/* Error message */}
      {errorMsg && (
        <p className="z-10 text-sm" style={{ color: '#ef4444' }}>{errorMsg}</p>
      )}

      {/* Spin number indicator */}
      {spinsRemaining > 0 && !isSpinning && spinNumber < 3 && (
        <p className="z-10 text-sm" style={{ color: '#a8a8b3' }}>
          Spin #{spinNumber + 1} of 3
          {spinNumber < 2 && <span className="ml-2" style={{ color: '#5a5a6e' }}>— this one won't win, but try anyway!</span>}
          {spinNumber === 2 && <span className="ml-2 font-semibold" style={{ color: '#22c55e' }}>— guaranteed WIN!</span>}
        </p>
      )}

      {/* Result modal */}
      {showResultModal && result && (
        <div
          className="spooky-modal-overlay fixed inset-0 z-50 flex items-center justify-center p-4"
          onClick={() => setShowResultModal(false)}
        >
          <div
            className="spooky-modal-card relative max-w-md w-full rounded-2xl p-6 text-center max-h-[90vh] overflow-y-auto"
            onClick={(e) => e.stopPropagation()}
            style={{
              borderColor: result.wonPrize ? '#22c55e' : '#3a3a4e',
            }}
          >
            <button
              onClick={() => setShowResultModal(false)}
              className="absolute top-2.5 right-3.5 bg-transparent border-none cursor-pointer"
              style={{ color: '#94a3b8', fontSize: '1.4rem', fontWeight: 'bold', lineHeight: 1 }}
            >
              {'\u2715'}
            </button>

            {result.wonPrize ? (
              <>
                <div
                  className="w-20 h-20 rounded-full mx-auto mb-4 flex items-center justify-center text-3xl"
                  style={{
                    background: 'radial-gradient(circle, #ef4444 0%, #4a0e0e 100%)',
                    boxShadow: '0 0 30px rgba(239,68,68,0.5)',
                    animation: 'spookyPulse 1.5s ease-in-out infinite',
                  }}
                >
                  {'\uD83C\uDF83'}
                </div>
                <h2 className="spooky-modal-title bloody-text" style={{ color: '#ef4444' }}>
                  {'\uD83C\uDF89'} YOU WON! {'\uD83C\uDF89'}
                </h2>
                <p className="spooky-modal-text">{promoCode?.label}</p>
                <div
                  className="rounded-xl py-4 px-6 mb-4 cursor-pointer"
                  onClick={handleCopy}
                  style={{
                    background: '#180928',
                    border: '2px dashed #22c55e',
                  }}
                >
                  <p className="text-xs uppercase tracking-wider mb-1" style={{ color: '#a8a8b3' }}>Your Promo Code</p>
                  <div className="flex items-center justify-center gap-3">
                    <span
                      className="text-2xl font-bold tracking-widest"
                      style={{ fontFamily: 'monospace', color: '#4ade80' }}
                    >
                      {promoCode?.code}
                    </span>
                    {copied ? (
                      <Check className="w-5 h-5" style={{ color: '#22c55e' }} />
                    ) : (
                      <Copy className="w-5 h-5" style={{ color: '#a8a8b3' }} />
                    )}
                  </div>
                  <p className="text-xs mt-1" style={{ color: '#5a5a6e' }}>
                    {copied ? 'Copied!' : 'Click to copy'}
                  </p>
                </div>
                <p className="text-xs mb-4" style={{ color: '#5a5a6e' }}>
                  Use this code at checkout in our store!
                </p>
                <button
                  onClick={() => setShowResultModal(false)}
                  className="spooky-modal-btn"
                  style={{ background: 'linear-gradient(135deg, #ef4444, #22c55e)' }}
                >
                  Claim My Prize {'\uD83C\uDFAF'}
                </button>
              </>
            ) : (
              <>
                <div
                  className="w-20 h-20 rounded-full mx-auto mb-4 flex items-center justify-center text-3xl"
                  style={{ background: 'radial-gradient(circle, #2a1a2e 0%, #05020a 100%)' }}
                >
                  {'\uD83D\uDC80'}
                </div>
                <h2 className="spooky-modal-title bloody-text" style={{ color: '#ef4444' }}>
                  NO LUCK! {'\uD83D\uDC7B'}
                </h2>
                <p className="spooky-modal-text">
                  {result.spinsRemaining > 0
                    ? `You have ${result.spinsRemaining} spin${result.spinsRemaining !== 1 ? 's' : ''} left. Try again!`
                    : 'Better luck next time!'}
                </p>
                <button
                  onClick={() => setShowResultModal(false)}
                  className="spooky-modal-btn"
                  style={{ background: 'linear-gradient(135deg, #ef4444, #7f1d1d)' }}
                >
                  Close
                </button>
              </>
            )}
          </div>
        </div>
      )}

      {/* Embedded CSS with Bloody Font, Animations, and Effects */}
      <style>{`
        @import url("https://fonts.googleapis.com/css2?family=Creepster&display=swap");

        .spooky-app {
          background: #030108;
          background-image:
            radial-gradient(circle at 50% 15%, rgba(239, 68, 68, 0.45) 0%, transparent 55%),
            radial-gradient(circle at 15% 85%, rgba(34, 197, 94, 0.3) 0%, transparent 50%),
            radial-gradient(circle at 85% 85%, rgba(168, 85, 247, 0.3) 0%, transparent 50%);
          color: #fff;
          min-height: 100vh;
        }

        /* 🩸 BLOODY TEXT EFFECT FOR TITLE */
        .bloody-text {
          font-family: "Creepster", cursive, sans-serif !important;
          color: #ff1e1e !important;
          text-transform: uppercase;
          letter-spacing: 3px;
          text-shadow: 
            0 0 5px #ff0000,
            0 0 12px #ff0000,
            0 0 25px rgba(255, 0, 0, 0.8),
            0 0 35px rgba(239, 68, 68, 0.6),
            2px 3px 2px #000000;
          animation: bloodPulse 2.5s ease-in-out infinite alternate;
        }

        @keyframes bloodPulse {
          0% {
            text-shadow: 
              0 0 5px #ff0000,
              0 0 10px #ff0000,
              0 0 20px rgba(255, 0, 0, 0.8),
              0 0 30px rgba(239, 68, 68, 0.6),
              2px 3px 2px #000;
          }
          100% {
            text-shadow: 
              0 0 8px #ff2222,
              0 0 18px #ff0000,
              0 0 30px rgba(255, 0, 0, 1),
              0 0 45px rgba(34, 197, 94, 0.5),
              2px 4px 3px #000;
          }
        }

        /* Floating spooky emojis */
        .spooky-float {
          opacity: 0.3;
          filter: drop-shadow(0 0 10px rgba(239, 68, 68, 0.8));
          animation: spookyFloatAnim 5s ease-in-out infinite alternate;
        }
        .bat-1 { top: 12%; left: 6%; font-size: clamp(2rem, 3.5vw, 3.2rem); animation-duration: 4s; }
        .bat-2 { top: 18%; right: 6%; font-size: clamp(2.2rem, 3.8vw, 3.5rem); animation-duration: 5.5s; }
        .ghost-1 { bottom: 18%; left: 8%; font-size: clamp(2.4rem, 4vw, 3.8rem); animation-duration: 6.5s; }
        .ghost-2 { top: 45%; right: 5%; font-size: clamp(2rem, 3.2vw, 3rem); animation-duration: 7s; }
        .jack-1 { bottom: 10%; right: 12%; font-size: clamp(2.5rem, 4.2vw, 4rem); animation-duration: 4.8s; }
        .jack-2 { top: 28%; left: 4%; font-size: clamp(2.2rem, 3.6vw, 3.2rem); animation-duration: 5.2s; }
        .skel-1 { bottom: 28%; right: 8%; font-size: clamp(2rem, 3.5vw, 3.2rem); animation-duration: 6s; }
        .smile-bg { top: 5%; left: 50%; transform: translateX(-50%); font-size: clamp(3rem, 5vw, 5rem); animation-duration: 3.5s; }

        @keyframes spookyFloatAnim {
          0% { transform: translateY(0px) rotate(-6deg); opacity: 0.2; }
          100% { transform: translateY(-22px) rotate(8deg); opacity: 0.45; }
        }

        /* Subtitle */
        .spooky-subtitle {
          color: #4ade80;
          font-size: clamp(0.72rem, 2.6vw, 1.1rem);
          text-shadow: 0 0 8px rgba(74, 222, 128, 0.6);
          font-weight: 600;
          margin-top: 4px;
        }

        /* Attempts badge */
        .attempts-badge {
          background: rgba(239, 68, 68, 0.18);
          border: 1px solid #ef4444;
          color: #fca5a5;
          padding: 6px 18px;
          border-radius: 20px;
          font-size: clamp(0.75rem, 2.2vw, 1rem);
          font-weight: bold;
          box-shadow: 0 0 10px rgba(239, 68, 68, 0.4);
        }

        /* Eye-ball pointer */
        .eye-ball {
          width: 26px;
          height: 26px;
          background: #ffffff;
          border-radius: 50%;
          border: 2px solid #ef4444;
          display: flex;
          justify-content: center;
          align-items: center;
          box-shadow: 0 0 10px #ef4444;
          animation: eyeBlink 4s ease-in-out infinite;
        }
        .eye-pupil {
          width: 10px;
          height: 10px;
          background: #7f1d1d;
          border-radius: 50%;
          border: 2px solid #000;
          animation: pupilLook 3s ease-in-out infinite;
        }
        @keyframes eyeBlink {
          0%, 90%, 100% { transform: scaleY(1); }
          93% { transform: scaleY(0.1); }
        }
        @keyframes pupilLook {
          0%, 100% { transform: translate(0, 0); }
          25% { transform: translate(-3px, -2px); }
          50% { transform: translate(3px, 0); }
          75% { transform: translate(0, 2px); }
        }

        /* Spin button */
        .spooky-spin-btn {
          background: linear-gradient(135deg, #ef4444, #22c55e);
          color: #fff;
          font-size: clamp(0.95rem, 3.5vw, 1.2rem);
          font-weight: 900;
          padding: 12px 28px;
          border: none;
          border-radius: 30px;
          cursor: pointer;
          box-shadow: 0 0 18px rgba(239, 68, 68, 0.6);
          transition: all 0.2s ease;
          width: 90%;
          max-width: 320px;
          text-transform: uppercase;
          letter-spacing: 1px;
        }
        .spooky-spin-btn:hover:not(:disabled) {
          transform: scale(1.03);
          box-shadow: 0 0 22px rgba(34, 197, 94, 0.8);
        }
        .spooky-spin-btn:active:not(:disabled) {
          transform: scale(0.97);
        }
        .spooky-spin-btn-disabled {
          background: #271428;
          color: #6b586e;
          cursor: not-allowed;
          box-shadow: none;
        }

        /* Modal */
        .spooky-modal-overlay {
          background: rgba(3, 1, 8, 0.92);
          backdrop-filter: blur(6px);
          animation: spookyFadeIn 0.3s ease-out;
        }
        .spooky-modal-card {
          background: #0f051a;
          border: 2px solid;
          box-shadow: 0 0 25px rgba(34, 197, 94, 0.4), 0 0 15px rgba(239, 68, 68, 0.3);
          animation: spookyPopIn 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        .spooky-modal-title {
          font-size: 1.6rem;
          margin-bottom: 8px;
          text-transform: uppercase;
          letter-spacing: 1px;
        }
        .spooky-modal-text {
          color: #e2e8f0;
          font-size: 0.95rem;
          margin-bottom: 16px;
          line-height: 1.4;
        }
        .spooky-modal-btn {
          display: block;
          width: 100%;
          color: white;
          font-weight: bold;
          font-size: 1rem;
          padding: 11px 14px;
          border-radius: 22px;
          border: none;
          cursor: pointer;
          transition: all 0.2s ease;
        }
        .spooky-modal-btn:hover {
          transform: scale(1.03);
        }
        .spooky-modal-btn:active {
          transform: scale(0.97);
        }

        /* Keyframes */
        @keyframes spookyFadeIn {
          from { opacity: 0; }
          to { opacity: 1; }
        }
        @keyframes spookyPopIn {
          from { opacity: 0; transform: scale(0.8); }
          to { opacity: 1; transform: scale(1); }
        }
        @keyframes spookyPulse {
          0%, 100% { transform: scale(1); opacity: 1; }
          50% { transform: scale(1.08); opacity: 0.85; }
        }

        /* Desktop sizing */
        @media (min-width: 768px) {
          .wheel-container {
            width: min(65vw, 52vh, 480px) !important;
            height: min(65vw, 52vh, 480px) !important;
          }
          .eye-ball { width: 32px; height: 32px; }
          .eye-pupil { width: 12px; height: 12px; }
          .spooky-spin-btn {
            max-width: 380px;
            padding: 16px 36px;
            font-size: 1.3rem;
          }
        }

        /* Low-height screens */
        @media (max-height: 620px) {
          .spooky-app {
            justify-content: flex-start;
            padding-top: 12px;
            padding-bottom: 24px;
          }
          .wheel-container {
            width: min(75vw, 45vh, 280px) !important;
            height: min(75vw, 45vh, 280px) !important;
          }
        }
      `}</style>
    </div>
  );
}
