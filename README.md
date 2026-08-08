# rinie.github.io

A blog about software architecture, systems thinking, and the boundary between physical and logical layers.
Exploring being a Weak link willing to **learn** that is Listen to actual Use **feedback**.

Published at [rinie.github.io](https://rinie.github.io).

---

## The Framework Visualised

![Gutenberg vs Semantic iceberg](assets/img/gutenberg_semantic_iceberg.svg)

Left iceberg: Unix/Node.js with a clean boundary at `fread/libc` — the semantic top ~20% is portable and moves freely to any cloud zone. Right iceberg: Java/.NET where semantic noise permeates every layer and the whole stack must move together.

---

## The Series

One hundred posts, one coherent framework applied at every layer from printing presses to parking cars.

Posts are drip-fed one per day via GitHub Pages. If you cannot wait — the `_posts/` folder in this repo contains everything. No spoiler warnings. The series is a git repo, not a Netflix show. The external resolver is right there in the URL bar.

| # | Post | Date | Core idea |
|---|---|---|---|
| 1 | [The Gutenberg/Semantic Model](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/) | May 14 | The foundational framework |
| 2 | [Def-Use, Lean Pull, and Why the User Does Not Sux](https://rinie.github.io/2026/05/16/def-use-lean-pull/) | May 16 | The human feedback dimension |
| 3 | [MDI versus Tabs: Why Microsoft Kept Getting It Wrong](https://rinie.github.io/2026/05/17/mdi-versus-tabs/) | May 17 | Window management as tribal Def |
| 4 | [CarPlay, Nokia, and the Certification Tribe](https://rinie.github.io/2026/05/18/carplay-nokia-certification-tribe/) | May 18 | Certification as tribal weapon |
| 5 | [Sonos, Intel, and Apple: When the Tribe Breaks the Product](https://rinie.github.io/2026/05/19/sonos-intel-apple-tribe/) | May 19 | Gutenberg tribe claiming semantic layer |
| 6 | [The Linux Paradox: A Gutenberg Kernel and a Semantic Desktop](https://rinie.github.io/2026/05/20/linux-kernel-desktop-paradox/) | May 20 | Use oracle versus no oracle |
| 7 | [YAML, JSON, and the Config File That Fights Back](https://rinie.github.io/2026/05/21/yaml-json-config-formats/) | May 21 | O(1) boundary principle |
| 8 | [DuckDB: The Gutenberg/Semantic Model Done Right](https://rinie.github.io/2026/05/22/duckdb-gutenberg-semantic/) | May 22 | The model done right + dual-track strategy |
| 9 | [Exceptions, Result/Option, and HTTP: Error Handling as Def-Use](https://rinie.github.io/2026/05/23/exceptions-result-option-http/) | May 23 | Errors as values versus control flow hijacking |
| 10 | [Your Music Survived Six Formats. Why Did Your Bank Account Number Stay Behind?](https://rinie.github.io/2026/05/24/music-survived-six-formats/) | May 24 | Semantic identity versus Gutenberg carrier |
| 11 | [Your Email Address Is Hostage. It Does Not Have to Be.](https://rinie.github.io/2026/05/25/email-address-hostage/) | May 25 | Own your domain, make the carrier replaceable |
| 12 | [The Boundary Has a Lifecycle](https://rinie.github.io/2026/05/26/boundary-lifecycle/) | May 26 | From Unix portability to WebAssembly |
| 13 | [UUIDs Are Not Names](https://rinie.github.io/2026/05/27/uuids-are-not-names/) | May 27 | Gutenberg identifiers leaking into the semantic layer |
| 14 | [Why IPv6's Def-Push Failed: NAT Was Not a Hack](https://rinie.github.io/2026/05/28/why-ipv6-def-push-failed/) | May 28 | Chesterton's Fence and load-bearing boundaries |
| 15 | [Competition Is Use-Pull. Monopoly Is Def-Push. Government Is Both.](https://rinie.github.io/2026/05/29/competition-use-pull-monopoly-def-push/) | May 29 | Exit as the Use signal transmission mechanism |
| 16 | [Moore's Law as an Architectural Principle](https://rinie.github.io/2026/05/30/moores-law-architectural-principle/) | May 30 | Portability as a basket option on unknown future improvements |
| 17 | [The Compiler That Knew Where It Was](https://rinie.github.io/2026/05/31/compiler-snr-evolution/) | May 31 | SNR in IRs — why keeping `{}` structure is free information the flat CFG destroys |
| 18 | [Worse Is Better Because the Gap Is Where Evolution Happens](https://rinie.github.io/2026/06/01/worse-is-better-gap-evolution/) | Jun 1 | Gabriel was right about worse beating better, wrong about why |
| 19 | [Ten Users Saying It Sux Means It Sux](https://rinie.github.io/2026/06/02/ten-users-saying-it-sux/) | Jun 2 | 90/<10/<1 population scaling and the Def-Push distortion |
| 20 | [Your Lights Don't Know Your Name](https://rinie.github.io/2026/06/03/bluetooth-matter-rdf-naming/) | Jun 3 | Bluetooth, Matter, and RDF all push Gutenberg identifiers across the boundary |
| 21 | [The Postman Reads the Envelope, Not the Letter](https://rinie.github.io/2026/06/04/postman-reads-envelope/) | Jun 4 | Books, pages, and Gutenberg 2.1 for a general audience |
| 22 | [The Postman Reads the Envelope, Not the Letter: How Gutenberg 2.1 Bounds Complexity](https://rinie.github.io/2026/06/05/postman-does-not-read-letter/) | Jun 5 | DNS once, routing per page, libc as the waterline, Chrome and VS Code as flexible seams |
| 23 | [Working on the Same Page](https://rinie.github.io/2026/06/06/working-on-the-same-page/) | Jun 6 | Web craft boundaries — HTML, CSS, JS, HTTP as clean Def-Use seams |
| 24 | [Deprecation Considered Harmful](https://rinie.github.io/2026/06/07/deprecation-considered-harmful/) | Jun 7 | Old stars fade. You do not outlaw them. Python 3, UTF-16, and the 16-bit bet. |
| 25 | [Hiding the Waterline Makes You Drown Without Knowing Why](https://rinie.github.io/2026/06/08/hiding-the-waterline/) | Jun 8 | Legibility at the boundary is a feature, not a leak |
| 26 | [The Brick That Sticks Out: Why {} Beats Indentation](https://rinie.github.io/2026/06/09/brick-that-sticks-out/) | Jun 9 | Scanning for landmarks is O(1). Taking measurements is O(n). |
| 27 | [Revisiting the Waterline: Small Fixes, Five Years Later](https://rinie.github.io/2026/06/10/revisiting-the-waterline/) | Jun 10 | Platform drift, LTS cadence, and why you never deploy on Fridays |
| 28 | [After 25 Years SQL Still Wins](https://rinie.github.io/2026/06/11/sql-still-wins/) | Jun 11 | DBA tweaking queries after the architect left — and why SQL survives |
| 29 | [Famous Last Words: 640K, 65536, and the Ceiling You Are Drawing Right Now](https://rinie.github.io/2026/06/12/famous-last-words/) | Jun 12 | The arrogance of ceilings, Moore's Law, and the room that has walls |
| 30 | [Don't Go Down With Your Iceberg](https://rinie.github.io/2026/06/13/dont-go-down-with-your-iceberg/) | Jun 13 | The 10% is the steering budget. Accept it. |
| 31 | [31 Days, One Framework, One Conversation](https://rinie.github.io/2026/06/14/31-days-one-framework/) | Jun 14 | One-month reflection on the series and the workflow that produced it |
| 32 | [Gutenberg: Your Next Phone Will Be a Different Make on a Different Carrier. Your Number Is Still Yours.](https://rinie.github.io/2026/06/15/gutenberg-your-next-phone/) | Jun 15 | The external resolver as the mechanism of iceberg transitions |
| 33 | [The Complaint Department Has Been Transferred to Another Dimension](https://rinie.github.io/2026/06/16/complaint-department-transferred/) | Jun 16 | Enshittification — when the resolver becomes a toll booth |
| 34 | [Going to the Gemba: Getting Your Feet Wet at the Waterline](https://rinie.github.io/2026/06/17/going-to-the-gemba/) | Jun 17 | The sticky note is a bug report. Watch without helping. |
| 35 | [I Didn't See the Bore-Out Coming. Don't Ask Me to Park Cars.](https://rinie.github.io/2026/06/18/bore-out-dont-ask-me-to-park-cars/) | Jun 18 | Bore-out, Marvin, and the missing resolver between capability and task |
| 36 | [Every 'Where Is the Close Button?' Is a Bug](https://rinie.github.io/2026/06/19/where-is-the-close-button/) | Jun 19 | Krug's Don't Make Me Think as Use-Pull interface design |
| 37 | [Nothing Is Confusing to Me: The Inmates Are Running the Asylum](https://rinie.github.io/2026/06/20/nothing-is-confusing-to-me/) | Jun 20 | Proximity blindness, Dancing Bears, and the two Steves |
| 38 | [Muddy Water(line)s, Shady User Experience](https://rinie.github.io/2026/06/21/muddy-waterlines-sux/) | Jun 21 | Dark patterns, Chrome, and the browser that became the server spy |
| 39 | [Ambiguity Is Not a Bug: Trust, Provenance, and the Resolver That Cried Wolf](https://rinie.github.io/2026/06/22/ambiguity-is-not-a-bug/) | Jun 22 | WhatsApp fraud, npm hijacks, QR codes — hiding ambiguity is the bug |
| 40 | [Do You Trust This Document? No, I Have to Read It First.](https://rinie.github.io/2026/06/23/wasm-sandbox-vs-policy/) | Jun 23 | WASM sandboxes versus security policies — one is engineering, one is hope |
| 41 | [Hitchhiking to 42: What We Already Knew](https://rinie.github.io/2026/06/24/hitchhiking-to-42/) | Jun 24 | The rode draad, the O(1) boundary, the external resolver, and a reminder |
| 42 | [42: You Still Need a Towel at the Waterline](https://rinie.github.io/2026/06/25/42-towel-at-the-waterline/) | Jun 25 | The Answer is 42. The Question is still being computed. Remember: bring your towel. |
| 43 | [The Intuition Had a Shape](https://rinie.github.io/2026/06/26/the-intuition-had-a-shape/) | Jun 26 | Breadth-first all the way down — from TurboC to SSA to a flat token tape |
| 44 | [Cheap Moves Instead of Silver Bullet](https://rinie.github.io/2026/06/27/cheap-moves-instead-of-silver-bullet/) | Jun 27 | Brooks was right — but aimed at the wrong layer. The leverage is at the fast layer. |
| 45 | [Worse Ages Better Than Perfect](https://rinie.github.io/2026/06/28/worse-ages-better-than-perfect/) | Jun 28 | The perfectly designed thing is perfect for yesterday. Simplicity at the seam is portability through time. |
| 46 | [Buffer Overflow Is Printing Outside the Page](https://rinie.github.io/2026/06/29/buffer-overflow-is-printing-outside-the-page/) | Jun 29 | Gutenberg errors tear the page. Semantic errors stay on it. Rust keeps the channels separate. |
| 47 | [Marvin Remembers the Serial Number of Every Car. The Barrier Only Reads the Plate.](https://rinie.github.io/2026/06/30/marvin-remembers-serial-number/) | Jun 30 | Hide the serial number, expose the plate. The resolver maps them when needed. |
| 48 | [Breadth-First Is How You Find the Seams](https://rinie.github.io/2026/07/01/breadth-first-is-how-you-find-the-seams/) | Jul 1 | Breadth-first holds a layer flat and shows where the pace changes. That change is the seam. |
| 49 | [The Watershed: From Mac OS to iOS to Android, and the Linux Thread Running Through All of It](https://rinie.github.io/2026/07/02/the-watershed/) | Jul 2 | 1991→2001→2007→2024: commodity catching up, the Doom moment, DuckDB on Android |
| 50 | [You Actually Move Faster Without Breaking Things](https://rinie.github.io/2026/07/03/you-actually-move-faster/) | Jul 3 | Schmiel the painter, the 386 move, slow and smooth compounds, you know the next turn |
| 51 | [Penny Recognised Woz: The 90% Signal in Every Room](https://rinie.github.io/2026/07/04/penny-recognised-woz/) | Jul 4 | Zaphod, Sheldon, Woz — three feedback loops. Penny and Trillian know where the towel is. |
| 52 | [The Billy With Opinions: Linux, Ubuntu, and the Waterline That Keeps Moving](https://rinie.github.io/2026/07/05/billy-with-opinions/) | Jul 5 | Debian as the stable Billy, Ubuntu as rafting, Android/ChromeOS taking what works |
| 53 | [Frameworks Move the Seam: Even Trains Don't Demand a Wider Track](https://rinie.github.io/2026/07/06/frameworks-move-the-seam/) | Jul 6 | OOP, UTF-16, Brunel's gauge — infra changes are very expensive |
| 54 | [Every Night, Fixed Zero Days](https://rinie.github.io/2026/07/07/every-night-fixed-zero-days/) | Jul 7 | Potholes compound. Sinkholes get filled. Move on. Nothing broken. Most is well. |
| 55 | [If You Can't Diff It, It's a Dead End Street](https://rinie.github.io/2026/07/08/if-you-cant-diff-it/) | Jul 8 | git as the non-semantic index. The Universal Tree Fallacy (UTF). Plant new trees, do not move the forest. |
| 56 | [Stuck in the Middle: The JVM, CIL, and the Billy That Had to Read the Book](https://rinie.github.io/2026/07/09/stuck-in-the-middle/) | Jul 9 | Stack-based not register-based. Objects not values. The CD case Billy that refused Blu-ray. WASM got it right. |
| 57 | [Should I Click? Trusting the Letter by the Envelope](https://rinie.github.io/2026/07/10/should-i-click/) | Jul 10 | The opaque URL removed the envelope on purpose. Your hesitation is the system working correctly. |
| 58 | [The Streaming Billy Is Not That Far Away](https://rinie.github.io/2026/07/11/streaming-billy/) | Jul 11 | URI is the VIN. URL is the plate. IP is the parking space. 10x cannot be predicted. NAT as the accidental seam. |
| 59 | [Fixing the Seams Across the Waterline: Higher Dikes Are Not Always the Solution](https://rinie.github.io/2026/07/12/fixing-the-seams-across-the-waterline/) | Jul 12 | NL-Alert, the Ahr valley, and the seam between knowing and acting. Not smarter — earlier lessons learned. |
| 60 | [Embracing the Web Instead of Fighting It](https://rinie.github.io/2026/07/13/embracing-the-web/) | Jul 13 | XML fought the web and lost. Markdown, Node.js, TypeScript, Electron embraced it. The 10x cannot be led — only learned at the Gemba. |
| 61 | [Knowing Your Craft: Adding an Architect Slows Down, Adding a Bricklayer Speeds Up](https://rinie.github.io/2026/07/14/knowing-your-craft/) | Jul 14 | Craft-based shearing layers. The bikeshed is always a Semantic dispute. Linus only says no to waste. |
| 62 | [More Than 10% Waste? That Was a Leap of Faith.](https://rinie.github.io/2026/07/15/more-than-10-percent-waste/) | Jul 15 | Lean waste validates the architect. Build, Measure, Learn. Check validates both Do and Plan. The plan that learned. |
| 63 | [The Brick That Could Not Tell If It Was in the Living Room or the Kitchen](https://rinie.github.io/2026/07/16/brick-that-could-not-tell/) | Jul 16 | Mach, Hurd, DBus — the microkernel ignored the O(1) context switch cost. Plan 9 got the seam right. Android got the granularity right. |
| 64 | [WiFi, the Browser and the Router Just Evolve. Matter and MQTT Try to Fix the User.](https://rinie.github.io/2026/07/17/wifi-browser-router-evolve/) | Jul 17 | Ten router transitions, nothing broke. RFLink, Domoticz, Home Assistant — the boring seams won. Evolution will tell who suxed less. |
| 65 | [There Are No Living Room Bricks](https://rinie.github.io/2026/07/18/living-room-bricks/) | Jul 18 | You live in rooms. You build walls. The room is not a collection of bricks — it emerges from their arrangement. First fix, second fix, only the ends reflect taste. |
| 66 | [Only Time Will Tell: Evolution Is Cleverer Than You Are](https://rinie.github.io/2026/07/19/evolution-cleverer-than-you/) | Jul 19 | Gall's Law and Orgel's Rule. Centrally designed protocols improve logarithmically; evolvable ones improve exponentially. Unix vs Multics, git vs CVS, HTML vs XML — the demo always favours the system that hasn't crossed over yet. |
| 67 | [Open the Hood to Check a Flat Tire: The Def/Use Granularity Mismatch](https://rinie.github.io/2026/07/20/open-the-hood-flat-tire/) | Jul 20 | E07 on a dishwasher vs a 747 cockpit — both correct for their audience. "Which tire" is the resolver nobody wired. The car merges two unrelated problems into one frightening escalation path. |
| 68 | [Just Page 15, Please: The A4 Pattern and Why You Need Pages](https://rinie.github.io/2026/07/21/page15-please-a4-pattern/) | Jul 21 | tar is stuck in the tape era. zip's central directory is the page-number trick, and it's why Office documents are zip files full of XML. Parquet does the same trick over HTTP range requests. |
| 69 | [Tires 3.0, Oil 12.0, but (M)Apps Stuck at 1.0](https://rinie.github.io/2026/07/22/tires-vs-infotainment-pace-layering/) | Jul 22 | Pace layering predicted infotainment would be the fastest layer in a car. It's the slowest, because tires got an open regulated seam decades ago and infotainment never got one at all. |
| 70 | [Penny Recognised Woz: Why the Expert in the Room Misses the Obvious](https://rinie.github.io/2026/07/23/penny-recognised-woz/) | Jul 23 | The physicists didn't recognise Wozniak. The waitress did. Expertise points your attention so hard at one layer you stop seeing the person in front of you. The user who can't parse your jargon is often seeing the room clearly. |
| 71 | [Brooks Named the Waterline in 1986](https://rinie.github.io/2026/07/24/brooks-named-the-waterline/) | Jul 24 | Essential vs accidental complexity. AI eats the accidental kind — 8x more code, 1.3x more releases. The bricks were never the bottleneck. If the check says the plan was wrong, 8x the bricks builds the wrong thing faster. |
| 72 | [Evolve or Jump Ship? Frameworks Come With an Iceberg Attached](https://rinie.github.io/2026/07/25/evolve-or-jump-ship/) | Jul 25 | A library you call; a framework calls you, and it comes with an iceberg — file structure, routing, build, deployment. The question before boarding isn't whether it's good. It's whether you can leave. |
| 73 | [It Is Always DNS: The Version Chain Nobody Built](https://rinie.github.io/2026/07/26/it-is-always-dns-version-chain/) | Jul 26 | DNS changes are all-or-nothing. No staged rollout, no last known good, no revert. A hijacked record looks identical to a legitimate update. Last known good plus a confirmation loop plus a 72-hour window would fix it — no protocol changes required. |
| 74 | [Don't Erase the White Line. Don't Pour Concrete Either.](https://rinie.github.io/2026/07/27/dont-hide-the-fence/) | Jul 27 | The seam between content and code should be findable without being obstructive. HTML gets it right at the file boundary. XML pours concrete. Markdown erases the line when it's load-bearing. Sinkhole UX is what happens when you remove the marking but not the edge. |
| 75 | [URL, DNS and the @ Sign: Email and the Web Kept the Global Layer Small and Externally Resolvable to IP](https://rinie.github.io/2026/07/28/at-sign-layer-boundary/) | Jul 28 | LDAP stored full identity in a location-lookup system. Email and the web kept the global layer small — just enough to resolve to an IP — and pushed everything else into the semantic layer. The @ sign marks the boundary. HTTP borrowed the same principle from MIME. |
| 76 | [com.google.gson Goes Nowhere: Hardcoded Names Are as Bad as Hardcoded Numbers](https://rinie.github.io/2026/07/29/java-reversed-hierarchy-forgot-resolver/) | Jul 29 | Java and .NET reversed DNS hierarchy for unique names, then stopped — no resolver. A hardcoded package name is structurally identical to a hardcoded IP: both skip the resolver and bind the Use to the Def's iceberg address. Modularity means the Use can replace the Def by another implementation. ES6 import maps get this right. |
| 77 | [Borg's Arrogance: We Will Just Recompile the Iceberg](https://rinie.github.io/2026/07/30/resolver-hardens-or-atrophies/) | Jul 30 | Intel owned the fab and the architecture until ARM climbed in from outside. Google's monorepo can make breaking changes atomic — every caller is in the same tree. Linux and Chrome can't see their callers, so they freeze APIs and only break for security. Leaders become laggards when they cannot move to a better iceberg that had a worse start. |
| 78 | [Why Does My "Smart" Coffee Machine Not Know My Usual Cuppa?](https://rinie.github.io/2026/07/31/smart-coffee-machine-usual-cuppa/) | Jul 31 | A barista knows your usual after three visits. A connected machine has two years of history, a camera, and a sensor suite — and still opens a blank menu at 07:00. The cup is already on the tray. YAGNI without the Gemba walk is just the Def side deciding for the Use side. |
| 79 | [Google Says Welcome Home. Your Car Just Sold You a Home Button.](https://rinie.github.io/2026/08/01/finished-at-the-point-of-sale/) | Aug 1 | Google Maps learned where you live by watching you. Your car's nav has a form field. The difference is whether the company thinks the product is finished at sale. BSH's oven is worse than Samsung's, not clumsier — structurally incapable of closing the loop. Street View shows the front door, never the windows. |
| 80 | [A Car You Do Not Drive](https://rinie.github.io/2026/08/02/car-you-do-not-drive/) | Aug 2 | A hi-rail truck has road wheels and rail wheels. On the road the driver resolves every intersection live. On the rails, steering disappears — the path was fixed the day the track was laid. Same vehicle, same driver, two completely different relationships to the seam. |
| 81 | [The Meterkast Pattern: Your Router Is a Utility Cabinet](https://rinie.github.io/2026/08/03/the-meterkast-pattern/) | Aug 3 | Every Dutch house has a meterkast — one cabinet where grid, water, gas, and sewer all cross into private space, each metered. Your router's NAT gateway does the same job — swap ISPs and nothing inside notices. Stewart Brand's shearing layers have no fence for this crossing at all. |
| 82 | [Cloud Breaks the Pace Layers: When Copying Site Gets Cheap](https://rinie.github.io/2026/08/04/cloud-breaks-the-pace-layers/) | Aug 4 | Brand's Site sits at the slow end because moving a foundation is expensive. Cloud made copying an entire environment cheap — minutes, not months. Rollback to last known good stops being a rebuild and becomes a routing decision. Gartner's Systems of Record inherited a cost assumption that no longer holds. |
| 83 | [Your Oven Gets One Chance Every Fifteen Years](https://rinie.github.io/2026/08/05/purchase-frequency-gates-learning/) | Aug 5 | A phone company gets fresh purchase feedback every two years. An oven gets one every fifteen. Fuel and tyres never even reach the manufacturer — third-party touchpoints, high frequency, zero feedback. The car's infotainment ages like a phone but sits in a car's purchase cycle with no replacement channel — exactly why CarPlay won. |
| 84 | [Wirth's Law Only Holds If You Recompile the Whole Iceberg](https://rinie.github.io/2026/08/06/wirths-law-beaten-by-android/) | Aug 6 | Three ways to waste the hardware gains: the log cabin (JVM, .NET, Electron), the walled garden (Pascal, Modula, Ada — real modularity, no external resolution), and real-world use (Android's ART, Hejlsberg's TypeScript, and his 2025 choice of Go over Rust for TS7's own compiler). Ada earned its walls; Rust is the modern proof safety and resolution aren't in tension. |
| 85 | [Taste Comes Last: The Electrician Doesn't Care What Color Your Switch Is](https://rinie.github.io/2026/08/07/taste-comes-last/) | Aug 7 | The junction box and the pipe thread are standard interfaces fixed decades before any given build, letting taste be decided last without blocking any trade. No committee designed them — they're the residue of a century of friction settling. No model, no architect, just real-world use. |
| 86 | [Use-Specialisation: The Gutenberg Pipe Stays Wide, the Use Side Narrows](https://rinie.github.io/2026/08/08/use-specialisation/) | Aug 8 | The Def's job is to keep the pipe wide and never force the Use side to specialise early. SELECT * EXCLUDE/RENAME, append-only parameters, HTTP redirects, opaque handles, and Parquet's partition pruning plus byte-range requests are all the same pipe. A resolver watching one consumer's repeated narrowing can learn it as taste, the way a barista learns your usual. |
| 87 | [Every Def-Push Playlist Resolver Gets Replaced When They Take You for a Ride — I Can Already Drive My Phone Number](https://rinie.github.io/2026/08/09/playlist-resolver-take-for-a-ride/) | Aug 9 | Google let you find pages. Spotify let you get music — before that, iTunes, MP3, CD. Netflix let you get movies — before that, DVD. Every one was Def-Push, and every Def-Push resolver eventually takes its users for a ride. Number portability never gets replaced, because I already drive it — nobody else was ever in the seat. |
| 88 | ["Obsolescence Was Not an Issue": When the Cloud Dies on Purpose](https://rinie.github.io/2026/08/10/obsolescence-was-not-an-issue/) | Aug 10 | Acura's 4G hardware still worked when they shut off AcuraLink anyway — their own words. That sentence separates a real Gutenberg constraint (3G sunset) from a pure Def-Push choice dressed as inevitability. The OBD port was a real standard the whole time; nobody built a safety-feature contract on top of it. |
| 89 | [Aggregation Theory: Modularize the Supplier, Integrate the Customer](https://rinie.github.io/2026/08/11/aggregation-theory-modularize-supplier/) | Aug 11 | Ben Thompson's 2015 framework read through the series' own vocabulary: the aggregator is a resolver with a business model, modularising suppliers into interchangeable Gutenberg-layer commodities while integrating an exclusive Semantic-layer customer relationship. It also explains why aggregators eventually become the toll booth. |
| 90 | [Architecture Astronauts: Solving the Template of a Problem Instead of the Problem](https://rinie.github.io/2026/08/12/architecture-astronauts/) | Aug 12 | Joel Spolsky coined the term watching Hailstorm die in 2001. Seven years later he watched the same team pitch the same architecture again as Live Mesh. The WAP coupon example says it best: solving the one problem the coffee shop doesn't have. |
| 91 | [Kanban Already Had the Words: Push and Pull, Before This Series Borrowed Them](https://rinie.github.io/2026/08/13/kanban-taught-def-push-use-pull/) | Aug 13 | This series' Def-Push/Use-Pull vocabulary traces to a 2015 Kanban article. Work assigned to a person is a push; work pulled from a shared backlog when someone's ready is a pull. Accountability attaches to the outcome, not the queue — and pull systems scale precisely because nobody owns a private backlog. |
| 92 | [We Hate Air: IKEA's Actual Design Philosophy, and Where the Billy Bookcase Came From](https://rinie.github.io/2026/08/14/ikea-oriented-development/) | Aug 14 | The real source behind this series' flatpack imagery. IKEA hates shipping empty space — the same cost a SELECT * pays in unfetched columns. Stick to screws you can assume everyone owns rather than allen keys you'd have to bundle. And disposable, ugly SQL beats precious, elegant alternatives for the same reason flatpack furniture is hackable. |
| 93 | [Users Don't Read, They Scan: Breadth-First, Confirmed Twenty-Three Years Later](https://rinie.github.io/2026/08/15/users-scan-breadth-first/) | Aug 15 | Nielsen found in 1997 that 79% of users scan rather than read. NN/g re-verified it themselves across 13 years and two writing systems — still true. Marketese carries a measured 27–124% usability cost. Two new scanning patterns emerged as pages got more complex, proof the behavior stayed constant while the layout changed. |
| 94 | [Perl 6 Promised Better. Python 3 Just Shipped.](https://rinie.github.io/2026/08/16/perl6-python3-walled-garden/) | Aug 16 | Paul Ford's 2015 Bloomberg essay noted, in passing, that Perl 6 had been fifteen years in the making with no ship date. Python 3 broke real things too, but kept a decade-long bridge back to Python 2. Same promise — a strictly better language — completely different outcome, because only one of them built a resolver. |
| 95 | [The Latency Table Already Knows the Gutenberg Layer Compounds and the Semantic Layer Doesn't](https://rinie.github.io/2026/08/17/latency-table-is-the-split/) | Aug 17 | Jeff Dean's 2012 latency table aged unevenly — SSD reads and memory bandwidth got 10-20x faster, but a packet from California to the Netherlands still takes exactly 150ms. Technology compounds, physics doesn't, and that one unmovable number is exactly why cloud regions and CDN edges exist. |
| 96 | [The Program Model Is the Def Model, and It's the One That Has to Move](https://rinie.github.io/2026/08/18/program-model-is-the-def-model/) | Aug 18 | Joel Spolsky's 2000 essay draws the Def/Use split without ever using those words. The user model is inherited before anyone touches your software; the program model is what the programmer built. When they conflict, Joel's rule is blunt — change the program model, because that's the only side still free to move. |
| 97 | [Fifty Million Users Cannot Hold It Wrong](https://rinie.github.io/2026/08/19/fifty-million-users-cannot-hold-it-wrong/) | Aug 19 | Steve Jobs tried to move the user model instead of the program model on the iPhone 4 — hold it differently, rather than fix the antenna. It didn't work. Three weeks later Apple shipped bumper cases, then redesigned the antenna in the next model. The grip was never the bug. |
| 98 | [The Migration Was Good. The Destination Is Still Debatable.](https://rinie.github.io/2026/08/20/python3-migration-good-destination-questionable/) | Aug 20 | Python 2 to 3's bridge was well built, but the destination is a separate claim — the actual breaking change was the str/bytes split, not the internal encoding, so UTF-8 internally would have cost exactly the same. Twelve years later CPython still hasn't switched, though its own core developers have an open issue proposing it. |
| 99 | [Below the Waterline, Identity Is a Label Too](https://rinie.github.io/2026/08/21/waterline-identity-is-a-label-too/) | Aug 21 | Zigbee2MQTT's network map takes minutes to render and means almost nothing when it finishes. There's a second waterline underneath the data one: raw keys (MAC, IP, hostname) are observations, not identity. A universal MQTT broker doesn't escape the one-canonical-tree mistake — it's the same tree, spelled with slashes instead of a schema. |
| 100 | [Neither the Tree Nor the Deluge: Why Ten Results and a 'Next Page' Button Won](https://rinie.github.io/2026/08/22/neither-tree-nor-deluge/) | Aug 22 | Yahoo's directory exposed the curator's own category tree. AltaVista's 40,000 results were a wide pipe with no narrowing at all. Google refused both — ten ranked results, everything else one click away behind a page number. Use-specialisation as a product decision, not just a coding pattern. |

Posts publish one per day via Jekyll's scheduled publishing. GitHub Pages publishes each post automatically when its date arrives at midnight UTC (02:00 Netherlands time), triggered by a daily GitHub Actions workflow.

---

## The Core Framework

**The Gutenberg layer** is physical and positional — bytes, blocks, pages, frames, IP addresses, sector offsets. Position and size matter. The medium is part of the artifact.

**The Semantic layer** is logical and meaningful — characters, words, chapters, hostnames, messages, rows. Hierarchy and meaning matter. Content is independent of the medium.

The name comes from Gutenberg's printing press: a physical process that fixes semantic content (text) onto a physical artifact (a page) at a specific position (a folio). The page number is Gutenberg. The chapter title is Semantic.

**Gutenberg 2.0** — Unix, TCP/IP, virtual memory. The bytestream abstraction hides the physical medium. Everything is a file.

**Gutenberg 2.1** — bytestream + UTF-8 + git. Portability extended to text and to software itself. Clone a repo, run on any machine.

The best systems isolate the Gutenberg/Semantic boundary in one place — a parser, a codec, a DNS resolver — and keep it clean.

---

## About This Series

A plain-English map of the framework, written for readers arriving without the full series context: [/about/](https://rinie.github.io/about/)

---

## Stack

- [Jekyll](https://jekyllrb.com) with the Minima theme
- Hosted on [GitHub Pages](https://pages.github.com)
- Posts written in Markdown
- Daily publish via GitHub Actions (`.github/workflows/daily-publish.yml`)
- No build step required locally — GitHub builds on push

---

## Adding a Post

Create a file in `_posts/` named `YYYY-MM-DD-title.md` with front matter:

```markdown
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
tags: [tag1, tag2]
level: general
description: "One or two sentence description for the index."
---

Post content here.
```

Push to `main`. GitHub Pages builds and publishes automatically. Future-dated posts publish at midnight UTC on their date without any further action.

---

## Assets

The iceberg diagram (`assets/img/gutenberg_semantic_iceberg.svg`) illustrates where you stand relative to the waterline. The house diagram (`assets/img/gutenberg_pace_layering_house.svg`) shows the full pace-layering model from hardware to human meaning. Both open in any browser or vector editor.
