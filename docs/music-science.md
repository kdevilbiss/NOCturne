# NOCturne and the Science of Accidental Music

*Why a stream of alerts comes out sounding like a song instead of a smoke detector.*

---

## Start with the McFerrin trick

In 2009, at a World Science Festival panel called *Notes & Neurons*, Bobby McFerrin did a demo that neuroscientists still cite. He stood in front of an audience of a few thousand strangers, sang a note, and hopped to a spot on the stage. New spot, new note. Within about thirty seconds the audience understood the game: the stage was a keyboard and his body was the cursor. Then he jumped to a spot he had never assigned a note to.

The entire audience sang the right pitch. A note he never taught them. He kept going, jumping around the stage, and a few thousand people with no rehearsal sang melodies together on the first try. His point, as he put it: "regardless of where I am, anywhere, every audience gets that."

What the audience was doing, without knowing it, was singing the **pentatonic scale**. They didn't know the next note because they were musicians. They knew it because pentatonic patterns are so deeply worn into human hearing, across essentially every culture on Earth, that the brain autocompletes them.

NOCturne is a bet on that same pre-install. It never tries to make alerts musical. It makes it *impossible for them to be unmusical*, then lets randomness wander around inside what's left. Everything below is just the details of how that cage is built.

---

## 1. Pitch is just speed

A musical note is a vibration, and pitch is how fast it vibrates. NOCturne's reference pitch is **55 Hz**, the note A1, which means a speaker cone pushing air back and forth 55 times a second. That's the `rootHz` in the config, and the whole track is tuned from it.

Two facts about how ears work do a lot of heavy lifting:

- **Doubling the speed gives you the "same" note, higher.** 110 Hz, 220 Hz, and 440 Hz are all A. The brain hears doubled frequency as identity, not difference. That doubling is an **octave**.
- Western tuning slices each octave into **12 equal steps** called semitones. Each semitone multiplies frequency by 2^(1/12), about 1.0595. Twelve of those multiplications and you've exactly doubled: you're home, one octave up.

So all of pitch space is a ladder with 12 rungs per octave. Every melody you've ever heard is a path along that ladder.

## 2. Why some notes fight

If any 12 rungs were as good as any other, music would be easy. They aren't. When two notes play together, their vibrations either mesh or grind, and the difference comes down to ratios.

Simple ratios mesh. An octave is 2:1. A perfect fifth (7 semitones, A up to E) is almost exactly 3:2, meaning every second wave of one note lines up with every third wave of the other. The ear reads that regular alignment as *consonance*: stable, open, calm.

Messy ratios grind. Two notes one semitone apart (a minor second) have waves that almost line up but keep slipping, which produces a physical beating sensation in the inner ear. Then there's the interval of 6 semitones, the **tritone**, historically nicknamed *diabolus in musica*, the devil in music. It's the sound horror movies reach for.

So the 12-rung ladder contains landmines. Pick two random rungs and you have decent odds of stepping on one. This is why "play random notes" sounds like a cat on a piano: not because randomness can't be musical, but because the full 12-note menu includes poison.

## 3. The pentatonic cheat code

Here's the move that makes NOCturne work, and it's the same move that makes wind chimes work, and McFerrin's audience work: **remove the landmines from the menu**.

A pentatonic scale keeps only 5 of the 12 notes. NOCturne's default, the minor pentatonic, keeps the rungs at 0, 3, 5, 7, and 10 semitones above the root. From A: the notes A, C, D, E, G.

Now do the math on every possible pair of those five notes. The intervals you can form are 2, 3, 4, 5, 7, 9, and 10 semitones. Check what's missing: **no minor second, no tritone**. The two grinding intervals literally cannot occur. Any two notes from this menu, played at any time, in any combination, land somewhere between "perfectly stable" and "mildly spicy."

That's the whole trick. NOCturne doesn't choose good notes. It made bad notes unreachable, which is a much easier engineering problem. Wind chime manufacturers tune their tubes pentatonically for exactly this reason: the wind is a random number generator, and the chime is the constraint that launders randomness into pleasantness.

NOCturne ships three menus, selectable as the Scale buttons:

| Mood | Semitones | Character |
| --- | --- | --- |
| Minor | 0, 3, 5, 7, 10 | The default. Bluesy, melancholy, safe from every angle. |
| Major | 0, 2, 4, 7, 9 | Same safety guarantee, sunnier vowels. |
| Dark | 0, 1, 5, 7, 8 | The deliberate rule-breaker. |

Look at Dark closely: it contains a 1. That's a minor second above the root, one of the landmines, reintroduced *on purpose*. This scale is close to the Japanese *miyako-bushi* scale you hear in koto music. One controlled half-step injects unease while the rest of the scale's wide gaps keep things from collapsing into noise. That's why Dark sounds tense rather than broken: it's one drop of poison, measured.

## 4. Time gets the same treatment as pitch

Pitch quantization alone gets you wind chimes: pleasant, aimless. The second constraint is rhythm, and it works on an identical principle: **snap a continuous value to a grid of allowed positions**.

NOCturne runs a clock of 16 steps per bar. At the default 96 BPM, a bar lasts 2.5 seconds and each step is about 156 milliseconds. When an alert's turn comes up, its note does not fire the instant the code processes it. It's booked into the next free slot on the grid.

Why this matters comes from a quirk of perception called **beat induction**. The brain treats events that align in time as *intentional* and events that don't as *accidental*. Two clicks at random offsets are noise. The same two clicks landing on a shared pulse are a rhythm, and your head starts nodding before you've made any conscious judgment. Alignment reads as agency. A drum machine's quantize button exploits this; so does NOCturne.

So an alert from a firewall in Singapore and an alert from a database in Ohio, which have no relationship whatsoever, arrive sounding *coordinated*, because both were snapped to the same grid. The listener's brain does the rest, inferring a drummer who does not exist.

## 5. The bed: context that turns notes into statements

Quantized pitch plus quantized time still isn't quite a song. The missing ingredient is **expectation**, and that's the job of the bed: the bass, pad, kick, and hat that play underneath everything.

A melody note has no meaning by itself. It has meaning *relative to what's under it*. The same E feels resolved over an A bass and restless over a G bass. Tension and release, leaving home and coming back, is the actual engine that makes a sequence of sounds feel like it's *going somewhere*.

NOCturne's bed loops a four-bar pattern of chord roots, `[0, 4, 2, 0]` in scale degrees: home, away, halfway back, home. The bass plays the root, and the pad plays the same degree two octaves up (in a five-note scale, +10 degrees is exactly two octaves, a nicely tidy coincidence of pentatonic arithmetic). Notice what the pad is *not* playing: a full chord. Just root and root, doubled high. Open harmony with no third in it means there's nothing for an incoming alert note to clash against. The kick lands on beats one and three, the hat on the offbeats, and that's the entire skeleton.

The consequence is subtle and important: an alert plays the same pitch every time (more on that next), but it *feels* different depending on which bar it lands in, because the floor under it moved. Free variety, no decisions required.

You can hear the bed's contribution directly: press **Hush**. The alert notes mute and the bed grooves on, perfectly fine but empty, a stage with nobody on it. Unmute and every alert suddenly lands *inside* something again. That difference is what context does.

## 6. Orchestration by severity

So far, every alert is interchangeable. The mapping layer is where monitoring data becomes *instrumentation*, and each severity gets a voice (all of it visible in the Reference panel, adjustable in Settings):

| Severity | Register | Waveform | Loudness | Length |
| --- | --- | --- | --- | --- |
| Warning | High (octave 3) | Sine | 0.22 | 0.4 s |
| Error | Middle (octave 2) | Triangle | 0.28 | 0.5 s |
| Critical | Low (octave 1) | Sawtooth | 0.34 | 0.7 s |

Each column is a psychoacoustic lever:

- **Register.** Across the animal kingdom, big things make low sounds. That size-to-pitch mapping is ancient and pre-verbal, which is why a critical alert rumbling an octave below everything else reads as *large* without anyone being told.
- **Waveform is timbre, and timbre is harmonics.** A sine wave is a single pure frequency, soft as a flute. A triangle wave adds faint odd harmonics, slightly more voiced. A sawtooth contains *every* harmonic, strong and bright: it's the raw material of brass, strings, and alarm buzzers. So criticals aren't just lower, they're *buzzier*, and buzzy plus low plus louder plus longer adds up to menace.
- **ACK and SDT alerts play at 0.4 times their normal loudness.** Handled noise steps back into the mix like backing vocals. You still hear that the work exists; it just stops demanding the spotlight, which is precisely what acknowledging an alert is supposed to mean.
- **Pan.** Each note also gets a stereo position based on its scale degree, low degrees left, high degrees right. It's the seating chart of an orchestra, and it keeps a dense mix from collapsing into a single point of mush.

## 7. Every device is a character

The pitch of an alert's note comes from hashing the resource name into one of the five scale degrees: `hash(name) % 5`. Deterministic, so **the same device always plays the same note**, forever.

This is the oldest trick in dramatic music: the *leitmotif*. Prokofiev's *Peter and the Wolf* gives every character an instrument and a theme, and after one introduction, children track the story by ear. Wagner ran entire operas on it. NOCturne gives every server a theme one note long, which sounds too small to matter until you've had the widget on for twenty minutes and notice you can identify the flapping interface *without looking*, because that pitch, on that beat density, is becoming familiar.

(It's also why the same device occupies the same patch of sky in the Cluster view: identity should survive across senses.)

## 8. Density: hearing load

The number of notes per bar follows the alert count through a tier curve: 2 notes per bar when quiet, 4 with a couple of alerts, 8 when things are busy, 12 in an incident. The pad also swells slightly when the count climbs past five.

This is the oldest sonification principle there is, the **Geiger counter principle**: encode magnitude as event rate. Nobody reads a number off a Geiger counter; the *rate of clicking* goes straight into your gut. Audition is uniquely good at this because hearing is pre-attentive: you process changes in tempo and density in the background, while your eyes and attention are somewhere else entirely. A dashboard demands that you look at it. A soundtrack tells you when to look.

That's the honest engineering justification hiding inside the joke: a quiet NOC hums, and when the hum starts crowding, you know about the incident before you've read a single alert. You heard the pressure change.

## 9. Why it occasionally writes a banger

Now the payoff question: why does a system this dumb sometimes produce a passage that sounds genuinely *composed*?

Brian Eno, who coined the term **generative music** and built *Music for Airports* on it, described the practice as planting a seed rather than writing a score: you design a system and constraints, set it in motion, and the music is whatever the system does. The composer's craft moves from choosing the notes to choosing the *rules*.

NOCturne's rules stack the odds like this:

- Pitch cannot leave the scale, so harmony is never wrong.
- Time cannot leave the grid, so rhythm is never wrong.
- The bed loops home every four bars, so structure is never absent.

Inside that cage, the actual sequence of notes is driven by things no composer controls: which devices are alerting, in what order, at what severity, at what count. Most of the time that random walk produces "perfectly fine." But melody has a shape your brain is hunting for, mostly **repetition plus small variation**, and a rotating list of fixed motifs over a looping bass line produces repetition-plus-variation *by construction*. So every so often the alert order, the density tier, and the bar root line up into a phrase that implies intention.

And then your brain commits the final act of composition. Humans are pattern-completing machines; we see faces in clouds and hear melodies in randomness whenever context primes us to. The bed is that priming. The difference between NOCturne and clouds is that this cloud is face-shaped far more often than chance, because everything unmusical was made unreachable in advance.

It's a slot machine that cannot lose, so occasionally it jackpots. When it does, the take usually survives a few more bars, because the rotation that produced it is still looping. That's what the Record button is for.

## 10. Hear each principle in the lab

Every claim above is checkable in `lab-nocturne.html` in about five minutes:

1. **Density (§8):** add a single alert with **+W** and listen. Clear, then Generate 250. Same machinery, completely different pressure.
2. **The bed (§5):** Toggle Hush both ways during a busy set. Notice the alert notes feel *placed* when the bed returns.
3. **Motif (§7):** Click the same chip several times. Same pitch, every time. Now click a different device.
4. **The landmine, controlled (§3):** Switch Scale from Minor to Dark mid-song. One half-step of menace enters the menu, and the mood changes without anything breaking.
5. **Quantized chaos (§4):** Press Storm. Eight criticals slam in, but on the next bar boundary, with a boom on the beat. Even the catastrophe is on time.
6. **The performer layer:** Turn on Auto-DJ and watch the Tone and Reverb sliders ride. That's the one human-shaped element, a filter sweep, layered over the generative core.

## Watch and read next

- **Bobby McFerrin, "The Power of the Pentatonic Scale"** (World Science Festival, *Notes & Neurons*, 2009). Three minutes. The audience is the instrument.
- **Brian Eno, "Generative Music"** (talk, 1996). The founding sermon: gardens, not architecture.
- **Leonard Bernstein, *The Unanswered Question*** (Harvard lectures, 1973). The deep end: six lectures on why music means anything at all.
- **Daniel Levitin, *This Is Your Brain on Music*** (2006). The accessible book on the perception side: expectation, beat induction, why your brain finishes phrases for you.

---

*NOCturne: alert noise you can dance to. The infrastructure was always singing; we just quantized it.*
