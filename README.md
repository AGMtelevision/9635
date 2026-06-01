import { useState, useEffect } from 'react';
import { Gift, Star, Users, Trophy, Clock, ChevronDown, MessageCircle } from 'lucide-react';

const styles = `
  @keyframes spin-wheel {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }
  .spin-wheel {
    animation: spin-wheel 6s linear infinite;
  }
`;

if (typeof document !== 'undefined') {
  const styleSheet = document.createElement('style');
  styleSheet.textContent = styles;
  document.head.appendChild(styleSheet);
}

const EVENT_END = new Date('2026-06-04T23:59:59');

function useCountdown(target: Date) {
  const [timeLeft, setTimeLeft] = useState(() => calcTimeLeft(target));

  useEffect(() => {
    const id = setInterval(() => setTimeLeft(calcTimeLeft(target)), 1000);
    return () => clearInterval(id);
  }, [target]);

  return timeLeft;
}

function calcTimeLeft(target: Date) {
  const diff = target.getTime() - Date.now();
  if (diff <= 0) return { days: 0, hours: 0, minutes: 0, seconds: 0 };
  return {
    days: Math.floor(diff / (1000 * 60 * 60 * 24)),
    hours: Math.floor((diff / (1000 * 60 * 60)) % 24),
    minutes: Math.floor((diff / (1000 * 60)) % 60),
    seconds: Math.floor((diff / 1000) % 60),
  };
}

const reviews = [
  {
    name: 'Thabo Nkosi',
    location: 'Johannesburg',
    text: 'This event is amazing! I invited three friends and already received my bonus. AGM APP is truly changing lives here in South Africa.',
    stars: 5,
    date: 'June 1, 2026',
  },
  {
    name: 'Nomvula Dlamini',
    location: 'Durban',
    text: 'I upgraded to G2 last week and the rewards came through immediately. Very trustworthy platform. Highly recommend to everyone!',
    stars: 5,
    date: 'June 1, 2026',
  },
  {
    name: 'Sipho Mahlangu',
    location: 'Pretoria',
    text: 'Shared with my whole family. My cousin already upgraded to G1 and we both got rewarded. This is a real opportunity!',
    stars: 5,
    date: 'June 2, 2026',
  },
  {
    name: 'Lerato Motsepe',
    location: 'Cape Town',
    text: 'The bonus system is transparent and payments are fast. I love how AGM APP supports South African workers with these incentives.',
    stars: 5,
    date: 'June 2, 2026',
  },
  {
    name: 'Bongani Zulu',
    location: 'Soweto',
    text: 'Never thought I could earn extra income this way. After joining through a friend I upgraded quickly and the draws are exciting!',
    stars: 5,
    date: 'June 3, 2026',
  },
  {
    name: 'Ayanda Cele',
    location: 'Port Elizabeth',
    text: 'AGM APP has great opportunities for everyone. The limited-time event is real and the rewards are worth it. Do not miss out!',
    stars: 5,
    date: 'June 3, 2026',
  },
];

function CountdownBlock({ value, label }: { value: number; label: string }) {
  const display = String(value).padStart(2, '0');
  return (
    <div className="flex flex-col items-center">
      <div className="bg-gradient-to-b from-orange-500 to-orange-700 text-white font-bold text-3xl md:text-4xl w-16 md:w-20 h-16 md:h-20 rounded-xl flex items-center justify-center shadow-lg shadow-orange-900/40 border border-orange-400">
        {display}
      </div>
      <span className="mt-2 text-orange-200 text-xs font-semibold uppercase tracking-widest">{label}</span>
    </div>
  );
}

function StarRating({ count }: { count: number }) {
  return (
    <div className="flex gap-0.5">
      {Array.from({ length: count }).map((_, i) => (
        <Star key={i} className="w-4 h-4 fill-orange-400 text-orange-400" />
      ))}
    </div>
  );
}

export default function App() {
  const time = useCountdown(EVENT_END);
  const [openFaq, setOpenFaq] = useState<number | null>(null);

  return (
    <div className="min-h-screen bg-neutral-950 text-white font-sans">
      {/* Header */}
      <header className="sticky top-0 z-50 bg-neutral-950/95 backdrop-blur border-b border-orange-900/40">
        <div className="max-w-5xl mx-auto px-4 py-3 flex items-center justify-between">
          <div className="flex items-center gap-2">
            <div className="w-9 h-9 rounded-lg bg-gradient-to-br from-orange-400 to-orange-600 flex items-center justify-center font-black text-white text-sm shadow">
              AGM
            </div>
            <span className="font-bold text-white text-lg tracking-tight">AGM APP</span>
          </div>
          <div className="text-orange-400 text-sm font-semibold flex items-center gap-1.5">
            <Clock className="w-4 h-4" />
            June 1–4 2026
          </div>
        </div>
      </header>

      {/* Hero */}
      <section className="relative overflow-hidden bg-gradient-to-br from-orange-950 via-neutral-900 to-neutral-950 pt-16 pb-12 px-4">
        <div className="absolute inset-0 opacity-10" style={{backgroundImage:'radial-gradient(circle at 70% 30%, #f97316 0%, transparent 60%)'}} />
        <div className="max-w-5xl mx-auto text-center relative z-10">
          <div className="inline-flex items-center gap-2 bg-orange-500/20 border border-orange-500/40 rounded-full px-4 py-1.5 mb-6">
            <span className="w-2 h-2 rounded-full bg-orange-400 animate-pulse" />
            <span className="text-orange-300 text-sm font-semibold uppercase tracking-widest">Limited-Time Event</span>
          </div>
          <h1 className="text-4xl md:text-6xl font-black leading-tight mb-4">
            <span className="text-white">Limited-time </span>
            <span className="text-orange-400">invitation</span>
            <span className="text-white"> event</span>
          </h1>
          <p className="text-neutral-400 text-lg md:text-xl max-w-2xl mx-auto leading-relaxed mb-10">
            To expand AGM APP's business in South Africa, AGM will be hosting an incentive event to recognize the hard work and contributions of all employees.
          </p>

          {/* Countdown */}
          <div className="bg-neutral-900/80 border border-orange-900/50 rounded-2xl inline-block px-8 py-6 mb-4">
            <p className="text-orange-400 text-sm font-bold uppercase tracking-widest mb-4">Event Ends In</p>
            <div className="flex items-center gap-3 md:gap-5">
              <CountdownBlock value={time.days} label="Days" />
              <span className="text-orange-500 text-3xl font-bold mb-5">:</span>
              <CountdownBlock value={time.hours} label="Hours" />
              <span className="text-orange-500 text-3xl font-bold mb-5">:</span>
              <CountdownBlock value={time.minutes} label="Minutes" />
              <span className="text-orange-500 text-3xl font-bold mb-5">:</span>
              <CountdownBlock value={time.seconds} label="Seconds" />
            </div>
          </div>
          <p className="text-neutral-500 text-sm">Event Period: June 1–4, 2026</p>
        </div>
      </section>

      {/* Intro paragraph */}
      <section className="max-w-3xl mx-auto px-4 py-10 text-center">
        <p className="text-neutral-300 text-base md:text-lg leading-relaxed">
          We encourage everyone to participate and share this opportunity with family and friends, contributing to the company's growth, generating more income, and creating a better life for their families.
        </p>
      </section>

      {/* Lucky Draw Wheel */}
      <section className="bg-gradient-to-b from-neutral-950 to-orange-950/20 py-12 px-4">
        <div className="max-w-5xl mx-auto text-center">
          <div className="flex items-center justify-center gap-2 mb-2">
            <Trophy className="w-5 h-5 text-orange-400" />
            <h2 className="text-2xl md:text-3xl font-black text-white">Lucky Draw Prizes</h2>
          </div>
          <p className="text-orange-400 font-bold text-lg mb-8">Maximum Reward: <span className="text-2xl">R10,000</span></p>

          <div className="flex flex-col md:flex-row items-center justify-center gap-10">
            <div className="relative group">
              <div className="absolute -inset-3 rounded-full bg-orange-500/20 blur-xl group-hover:bg-orange-500/30 transition-all duration-500" />
              <img
                src="/image_2026-05-27_11-15-18.png"
                alt="AGM Lucky Draw Wheel"
                className="relative w-64 h-64 md:w-80 md:h-80 object-contain drop-shadow-2xl select-none pointer-events-none spin-wheel"
                draggable={false}
              />
              <div className="absolute bottom-0 left-1/2 -translate-x-1/2 translate-y-6">
                <span className="bg-neutral-900 border border-orange-700 text-orange-400 text-xs font-bold px-3 py-1 rounded-full whitespace-nowrap">
                  Display Only
                </span>
              </div>
            </div>

            <div className="grid grid-cols-2 gap-3 text-left max-w-xs w-full mt-6 md:mt-0">
              {['R20','R100','R30','R300','R50','R800','R80','R3000','R200','R10000'].map((prize) => (
                <div
                  key={prize}
                  className="bg-neutral-900 border border-orange-900/60 rounded-lg px-4 py-2 flex items-center gap-2"
                >
                  <Gift className="w-4 h-4 text-orange-400 shrink-0" />
                  <span className={`font-bold ${prize === 'R10000' ? 'text-orange-400 text-base' : 'text-white text-sm'}`}>{prize}</span>
                </div>
              ))}
            </div>
          </div>
        </div>
      </section>

      {/* Rewards Section */}
      <section className="py-14 px-4">
        <div className="max-w-5xl mx-auto">
          <div className="text-center mb-10">
            <h2 className="text-2xl md:text-3xl font-black text-white mb-2">Rewards Programme</h2>
            <p className="text-neutral-400">Earn rewards two ways — invite others or upgrade yourself</p>
          </div>

          <div className="grid md:grid-cols-2 gap-6">
            {/* Invitation Rewards */}
            <div className="bg-neutral-900 border border-orange-900/50 rounded-2xl p-6 relative overflow-hidden">
              <div className="absolute top-0 right-0 w-40 h-40 bg-orange-500/5 rounded-full -translate-y-1/2 translate-x-1/2" />
              <div className="flex items-center gap-3 mb-6">
                <div className="w-10 h-10 rounded-xl bg-orange-500/20 flex items-center justify-center">
                  <Users className="w-5 h-5 text-orange-400" />
                </div>
                <h3 className="text-xl font-black text-white">1. Invitation Rewards</h3>
              </div>
              <div className="space-y-4">
                <div className="bg-neutral-800 rounded-xl p-4 border border-orange-900/30">
                  <div className="flex items-start gap-3">
                    <span className="text-orange-400 font-black text-sm mt-0.5">(1)</span>
                    <div>
                      <p className="text-white font-semibold">Invite an intern to join G1</p>
                      <p className="text-orange-400 font-bold mt-1">Get Platform Bonus R54 + 1 Draw</p>
                    </div>
                  </div>
                </div>
                <div className="bg-neutral-800 rounded-xl p-4 border border-orange-900/30">
                  <div className="flex items-start gap-3">
                    <span className="text-orange-400 font-black text-sm mt-0.5">(2)</span>
                    <div>
                      <p className="text-white font-semibold">Invite an intern to join G2</p>
                      <p className="text-orange-400 font-bold mt-1">Get Platform Bonus R216 + 2 Draws</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            {/* Intern Rewards */}
            <div className="bg-neutral-900 border border-orange-900/50 rounded-2xl p-6 relative overflow-hidden">
              <div className="absolute top-0 right-0 w-40 h-40 bg-orange-500/5 rounded-full -translate-y-1/2 translate-x-1/2" />
              <div className="flex items-center gap-3 mb-6">
                <div className="w-10 h-10 rounded-xl bg-orange-500/20 flex items-center justify-center">
                  <Trophy className="w-5 h-5 text-orange-400" />
                </div>
                <h3 className="text-xl font-black text-white">2. Intern Rewards</h3>
              </div>
              <div className="space-y-4">
                <div className="bg-neutral-800 rounded-xl p-4 border border-orange-900/30">
                  <div className="flex items-start gap-3">
                    <span className="text-orange-400 font-black text-sm mt-0.5">(1)</span>
                    <div>
                      <p className="text-white font-semibold">Upgrade to G1</p>
                      <p className="text-orange-400 font-bold mt-1">Get 1 Draw</p>
                    </div>
                  </div>
                </div>
                <div className="bg-neutral-800 rounded-xl p-4 border border-orange-900/30">
                  <div className="flex items-start gap-3">
                    <span className="text-orange-400 font-black text-sm mt-0.5">(2)</span>
                    <div>
                      <p className="text-white font-semibold">Upgrade to G2</p>
                      <p className="text-orange-400 font-bold mt-1">Get 2 Draws</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          {/* Note */}
          <div className="mt-6 bg-orange-950/40 border border-orange-700/40 rounded-xl p-5 flex items-start gap-3">
            <div className="w-6 h-6 rounded-full bg-orange-500 flex items-center justify-center shrink-0 mt-0.5">
              <span className="text-white text-xs font-black">!</span>
            </div>
            <p className="text-orange-200 text-sm leading-relaxed">
              <span className="font-bold">Note:</span> If you meet the requirements for the holiday event, please contact the hiring manager to apply for the reward.
            </p>
          </div>
        </div>
      </section>

      {/* FAQ */}
      <section className="py-10 px-4 bg-neutral-900/50">
        <div className="max-w-3xl mx-auto">
          <h2 className="text-2xl font-black text-white text-center mb-8">Frequently Asked Questions</h2>
          {[
            { q: 'Who can participate in this event?', a: 'All AGM APP employees and interns in South Africa can participate. You can also invite family and friends to join as interns.' },
            { q: 'When does the event end?', a: 'The event runs from June 1 to June 4, 2026. Make sure to complete your invitations and upgrades before the deadline.' },
            { q: 'How do I claim my reward?', a: 'Once you meet the requirements, contact your hiring manager directly to apply for your reward.' },
            { q: 'What is the maximum prize?', a: 'The lucky draw maximum reward is R10,000. Multiple draws increase your chances of winning higher prizes.' },
          ].map((item, i) => (
            <div key={i} className="border-b border-neutral-800 last:border-0">
              <button
                className="w-full flex items-center justify-between py-4 text-left"
                onClick={() => setOpenFaq(openFaq === i ? null : i)}
              >
                <span className="text-white font-semibold pr-4">{item.q}</span>
                <ChevronDown className={`w-5 h-5 text-orange-400 shrink-0 transition-transform duration-300 ${openFaq === i ? 'rotate-180' : ''}`} />
              </button>
              {openFaq === i && (
                <p className="pb-4 text-neutral-400 text-sm leading-relaxed">{item.a}</p>
              )}
            </div>
          ))}
        </div>
      </section>

      {/* Reviews */}
      <section className="py-14 px-4">
        <div className="max-w-5xl mx-auto">
          <div className="text-center mb-10">
            <div className="flex items-center justify-center gap-2 mb-2">
              <MessageCircle className="w-5 h-5 text-orange-400" />
              <h2 className="text-2xl md:text-3xl font-black text-white">What South Africans Are Saying</h2>
            </div>
            <p className="text-neutral-400">Real feedback from AGM APP members across South Africa</p>
          </div>
          <div className="grid sm:grid-cols-2 lg:grid-cols-3 gap-5">
            {reviews.map((r, i) => (
              <div key={i} className="bg-neutral-900 border border-neutral-800 rounded-2xl p-5 flex flex-col gap-3 hover:border-orange-900/60 transition-colors duration-300">
                <StarRating count={r.stars} />
                <p className="text-neutral-300 text-sm leading-relaxed flex-1">"{r.text}"</p>
                <div className="flex items-center justify-between mt-1">
                  <div>
                    <p className="text-white font-bold text-sm">{r.name}</p>
                    <p className="text-neutral-500 text-xs">{r.location}</p>
                  </div>
                  <p className="text-neutral-600 text-xs">{r.date}</p>
                </div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* CTA Banner */}
      <section className="py-12 px-4 bg-gradient-to-r from-orange-950 via-orange-900/80 to-orange-950">
        <div className="max-w-3xl mx-auto text-center">
          <h2 className="text-2xl md:text-3xl font-black text-white mb-3">Don't Miss This Opportunity</h2>
          <p className="text-orange-200 mb-6">Contact your hiring manager today to participate and claim your rewards before June 4, 2026.</p>
          <div className="inline-flex items-center gap-2 bg-orange-500 hover:bg-orange-400 transition-colors rounded-full px-8 py-3 text-white font-bold text-base shadow-lg shadow-orange-900/50 cursor-default">
            <Clock className="w-5 h-5" />
            Event Ends June 4, 2026
          </div>
        </div>
      </section>

      {/* Footer */}
      <footer className="bg-neutral-950 border-t border-neutral-900 py-8 px-4">
        <div className="max-w-5xl mx-auto flex flex-col md:flex-row items-center justify-between gap-4">
          <div className="flex items-center gap-2">
            <div className="w-8 h-8 rounded-lg bg-gradient-to-br from-orange-400 to-orange-600 flex items-center justify-center font-black text-white text-xs shadow">
              AGM
            </div>
            <span className="text-neutral-400 text-sm">AGM APP — South Africa</span>
          </div>
          <p className="text-neutral-600 text-xs text-center">
            Event Period: June 1–4, 2026 &nbsp;|&nbsp; For queries, contact your hiring manager
          </p>
          <p className="text-neutral-700 text-xs">© 2026 AGM APP. All rights reserved.</p>
        </div>
      </footer>
    </div>
  );
}
