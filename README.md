# Two-Word Terms
Some word lists for terms made of two words (joined using a space, hyphen, or nothing). [Apparently](https://www.grammarly.com/blog/open-and-closed-compound-words/) these are called "compound words", but these lists are specifically for terms made of *two* words (so "mother-in-law" is not included, for example).

## Files:

* **hyphens-real-world-usage.json**: Top 100k hyphenated terms based on real-world usage (by crawling a corpus of book). You'll probably want to only grab the first ~10k items if you're looking for commonly-hyphenated terms. Each item is a 2-element array containing the term, and then the number of occurrances in the corpus.
* **space.txt** ~3k terms composed of 2 space-separated words. They are from [this](https://github.com/matthewreagan/WebstersEnglishDictionary) Webster list.
* **hyphen.txt**: ~3k hyphenated words. They are from [this](https://github.com/matthewreagan/WebstersEnglishDictionary) Webster list.
* **nothing**: ~1.5k unhyphenated compound words, scraped from [here](https://www.enchantedlearning.com/wordlist/compoundwords.shtml).

## Corpus-Parsing Code

Here's the code I used to parse the book corpus, mainly for my future reference:

```js
async function crawlDirectory(directoryHandle, files) {
  for await (let [name, handle] of directoryHandle.entries()) {
    if(handle.kind === "file") files.push(handle);
    else if(handle.kind === "directory") crawlDirectory(handle, files);
  }
}

let directoryHandle = await window.showDirectoryPicker(); // choose the directory containing the text files
let files = [];
await crawlDirectory(directoryHandle, files);

let hyphenatedWordsHist = {};
for(let fileHandle of files.slice(0, 50000)) {
  let file = await fileHandle.getFile();
  let text = await file.text();
  let hyphenatedWords = [...text.toLowerCase().matchAll(/\s([a-z]+-[a-z]+)[\s?!.]/g)].map(m => m[1]);
  hyphenatedWords.forEach(w => hyphenatedWordsHist[w] = (hyphenatedWordsHist[w] || 0) + 1);
}

let top100k = Object.entries(hyphenatedWordsHist).sort((a,b) => b[1]-a[1]).slice(0, 100000);

console.log(top100k);
```

## hyphens-real-world-usage.json Examples By Index

To help you understand where you might want to cut off the `hyphens-real-world-usage.json` list, here are some examples pulled from the list at index increments of 1000:

```txt
    0: so-called, long-term, twenty-five, twenty-four, well-known, old-fashioned, e-mail, nineteenth-century, medium-high, well-being
 1000: war-torn, full-body, two-minute, eighty-seven, self-understanding, split-second, tex-mex, re-established, one-story, day-old
 2000: spot-on, self-regulating, five-foot, ankle-length, mixed-up, nu-i, french-style, half-circle, good-humored, day-trip
 3000: twice-weekly, dodd-frank, six-story, forty-minute, white-bearded, us-based, ankh-morpork, off-campus, single-sex, quelques-unes
 4000: fluid-filled, far-seeing, human-induced, sing-along, pedestrian-only, wine-producing, life-span, chicago-based, long-limbed, double-take
 5000: west-central, medium-grain, industry-wide, mud-caked, jump-started, slow-release, eight-legged, sixty-third, code-breaking, all-union
 6000: red-letter, lithium-ion, ten-story, mind-independent, anti-racism, four-armed, triangular-shaped, self-abasement, pile-up, stone-lined
 7000: bell-bottom, risk-neutral, swept-back, alka-seltzer, ground-attack, cranberry-orange, tobacco-stained, non-citizen, ever-ready, battle-weary
 8000: nature-based, single-arm, mud-walled, demi-douzaine, after-death, red-striped, milk-fed, black-capped, green-clad, life-insurance
 9000: pierre-simon, stiff-armed, fire-eaters, t-bills, inter-racial, half-dragged, greenish-brown, honey-brown, side-stepping, winn-dixie
10000: mid-west, us-mexico, now-forgotten, high-potency, red-velvet, lighter-weight, two-sentence, crystal-blue, inner-directed, non-fictional
11000: roll-on, half-demon, spleen-qi, g-rated, under-resourced, shallow-draft, great-grandchild, bose-einstein, sun-splashed, gem-like
12000: non-vegan, well-camouflaged, highest-priority, top-five, reagan-bush, less-experienced, line-caught, cross-town, hardware-based, four-fingered
13000: umami-rich, arm-chairs, out-thrust, thai-inspired, heavily-armed, deep-tissue, pot-holed, re-equip, well-spring, fee-only
14000: race-baiting, locked-room, design-time, nazi-era, well-draining, autosomal-recessive, koi-san, down-town, pressed-tin, blood-shot
15000: co-axial, rhinestone-studded, rear-wheel, community-minded, hard-heartedness, double-strain, baseball-sized, anti-spam, vector-based, auto-suggestion
16000: fish-cleaning, ten-step, sex-toy, plate-sized, pair-bond, ex-policeman, non-conformists, electro-optical, f-pawn, intra-arterial
17000: pro-republican, three-bar, post-run, strong-room, call-sign, ground-shaking, daitoku-ji, oldest-known, oak-lined, mid-back
18000: sea-chest, plant-strong, hand-turned, three-deep, hormone-like, inter-varsity, widely-used, counter-current, bottom-feeders, employment-related
19000: tree-shaped, oscar-worthy, zero-knowledge, slip-sliding, pan-grilled, beach-side, class-bound, village-based, light-water, pro-reform
20000: three-pin, book-lovers, bee-eaters, re-equipping, multi-engine, ben-ari, river-crossing, pre-aryan, self-assemble, mcgill-queens
21000: long-tongued, ill-humored, turning-points, anti-life, four-wheeling, super-powerful, anti-prostitution, well-mounted, just-published, ninety-mile
22000: thirty-story, strong-jawed, single-hearted, self-object, po-po, non-constant, meat-stuffed, wine-related, cinema-going, horse-radish
23000: mont-tremblant, screen-lock, folded-arm, af-assist, tusk-anini, sea-land, shark-fin, passing-out, range-finding, sugar-filled
24000: same-named, issue-oriented, lower-profile, heart-beats, wood-handled, auto-scaling, double-pronged, r-values, beer-bottle, grand-looking
25000: geo-location, vitamin-packed, finger-food, dinner-dance, barn-door, drum-like, mean-square, friction-free, mountain-sized, four-roomed
26000: blue-backed, iron-work, right-flank, two-pounder, coffee-producing, eco-city, e-r, mustard-dill, anti-interventionist, edo-tokyo
27000: hot-plate, tommy-guns, fish-tailed, sno-park, modality-specific, madonna-like, lilac-coloured, gaff-rigged, patient-centred, mashed-potato
28000: neo-conservatism, pro-ed, sabbath-day, all-source, post-metaphysical, a-piece, gate-crashed, marxist-inspired, with-outen, problem-see
29000: indo-muslim, anglo-sikh, v-weapon, laissez-vous, mini-market, sex-ratio, multi-sport, serving-woman, hammer-headed, maple-cinnamon
30000: front-bench, mixed-effects, road-rage, industrial-era, heavy-weapons, bullock-cart, full-sleeved, single-chamber, duck-like, crow-like
31000: well-painted, heart-breakingly, dumb-waiter, gaunt-looking, pure-play, non-inflammatory, pleasure-boat, air-drop, light-wood, open-housing
32000: f-statistic, action-hero, weak-eyed, half-stumbled, friday-afternoon, slim-line, post-religious, spice-scented, beginner-friendly, long-dreaded
33000: protein-digesting, chezhou-lei, ju-hai, bottom-level, large-breed, well-cleaned, cellar-like, casa-museu, after-hour, voice-only
34000: swai-phillips, self-saucing, sleep-tousled, pineapple-coconut, chicken-pox, suis-moi, bee-like, mexican-origin, re-entrance, crossing-point
35000: micro-finance, now-common, apple-flavored, civil-liberties, movement-related, bitter-smelling, crime-busting, nighty-night, wild-wood, orange-white
36000: syro-phoenician, fence-mending, brain-scan, re-offending, present-value, pro-syrian, lower-budget, fast-bowling, consumer-focused, song-birds
37000: re-organize, pastel-shaded, nylon-string, this-here, coal-miners, one-shots, escher-like, sun-rise, jacques-emile, extra-good
38000: wool-blend, stark-raving, pre-symbolic, sentry-go, tart-tongued, regulation-size, business-government, fly-casting, wave-lashed, late-phase
39000: school-teaching, anglo-british, double-line, flat-ironed, voyons-nous, horror-show, born-agains, cross-device, hand-finished, repris-je
40000: gwei-djen, wing-coverts, colour-coding, department-issued, geo-politics, flour-thickened, model-like, post-humanism, bank-owned, voice-box
41000: ruby-studded, bird-life, jerk-offs, crimson-red, mit-trained, high-tariff, sea-people, all-seater, non-anglophone, brazilian-born
42000: freeze-framing, sentence-level, much-imitated, insurance-based, b-b, flag-bearers, cream-yellow, up-tilted, pro-austrian, note-takers
43000: six-pence, brown-edged, working-girl, sweat-free, veto-proof, brackish-water, plush-covered, concert-going, whisper-shouted, combat-trained
44000: reed-bed, grey-brick, blood-bond, pyramid-building, russet-red, feather-headed, violence-related, s-d, bias-sliced, earth-orbit
45000: black-lace, paint-peeled, ohio-class, pitter-pattered, fuzzy-haired, single-product, oompa-loompa, low-clearance, ebony-black, eocene-oligocene
46000: city-specific, out-front, value-producing, half-shattered, half-ready, hat-tricks, cotton-white, houdini-like, half-comic, body-slamming
47000: non-relatives, moscow-trained, fat-fingered, cross-licensing, high-jinks, moss-lined, credit-scoring, shade-tree, forest-trees, ski-lodge
48000: non-album, court-martialing, geo-spatial, photo-finish, corporate-speak, half-tracked, big-selling, kick-up, anti-japan, pond-side
49000: time-interval, wheat-coloured, simple-seeming, penny-sized, re-imposition, gtp-binding, croque-mort, war-club, post-feudal, near-beer
50000: lung-cancer, ethnic-specific, cut-purse, fog-covered, cloth-capped, strange-tasting, chest-first, anti-radar, watt-evans, wheat-containing
51000: campbell-mcbride, motif-index, steel-dust, half-giants, rest-style, nation-forming, li-hua, fc-causa, port-channel, sammi-lou
52000: gut-clenching, near-accident, bored-sounding, marriage-day, herbal-based, stripped-pine, listening-in, gambier-parry, lance-sergeant, ba-boom
53000: tax-preparation, ace-king, money-wages, post-heideggerian, radish-like, ek-sistence, third-holiest, martinez-alier, non-genital, anti-athenian
54000: fairfax-lacy, des-ka, bapu-ji, jolly-goodday, cloud-claudia, small-capitalization, pancake-shaped, sugar-sand, football-size, raspberry-lemon
55000: mold-free, cookie-making, school-building, zeroth-order, sub-layer, high-dependency, gooding-williams, lead-line, arduino-based, razor-backed
56000: drink-fuelled, push-bike, energy-use, corner-shop, male-like, quality-driven, star-dusted, blue-velvet, harsh-voiced, gilt-edge
57000: muzzy-headed, ten-armed, twice-removed, gutenberg-richter, road-construction, lembre-se, mega-popular, ex-gang, multi-actor, shadow-shape
58000: ring-binder, al-khayr, el-hage, cia-run, transfusion-associated, d-penicillamine, androgen-dependent, three-compartment, sense-consciousness, tussie-mussie
59000: vitamin-c, strategic-planning, hyper-awareness, candy-red, card-based, mate-choice, self-construals, summer-fall, foam-padded, wound-down
60000: ise-shi, teacher-researchers, tui-chin, cross-term, pan-con, shar-lon, city-ships, gold-tufted, af-f, light-presence
61000: penned-up, re-painted, lieutenant-captain, anti-freudian, division-size, pre-combat, al-mawardi, sense-giving, fast-melting, benevolent-looking
62000: power-projection, referir-se, mente-cuerpo, arab-syrian, brown-orange, noelle-neumann, graeco-macedonian, dried-grape, altar-pieces, saintes-maries
63000: short-rotation, clap-clap, gun-making, re-charge, hair-styles, quasi-industrial, fogged-in, class-rooms, hole-nesting, land-birds
64000: energy-wasting, post-auschwitz, local-feeling, e-banking, pan-human, stuart-smith, faux-tudor, re-emphasise, house-plants, piano-key
65000: grog-shop, pro-socialist, ultra-easy, wide-bellied, wild-collected, pre-charge, work-stained, square-footage, steel-workers, high-nutrition
66000: half-humans, soient-elles, cayley-hamilton, j-town, human-heartedness, tablet-woven, beijou-o, mid-ming, rose-white, richard-lenoir
67000: bar-selehm, eight-position, gramm-latta, heat-processing, sky-plane, toddy-bob, li-jin, default-gateway, graphene-cnt, panzer-brigade
68000: water-dog, blanc-mange, pay-cable, aulu-gelle, quasi-orthogonal, marguerite-louise, lauderdale-hollywood, grand-case, two-drug, bont-a
69000: chamar-me, explicar-lhe, d-town, chance-determined, flying-fox, al-ittihad, end-perform, self-reconfigurable, micro-greens, bargained-for
70000: quasi-platonic, kannada-speaking, per-head, beta-decay, chemical-biological, adult-friendly, car-insurance, high-modulus, withered-looking, complete-data
71000: volcano-shaped, parent-adolescent, photo-realism, three-parent, old-family, sun-synchronous, stock-making, spruce-covered, person-based, type-two
72000: slime-green, eight-strong, spanish-built, moo-cow, widow-burning, mile-end, over-driven, cherry-plum, arms-smuggling, wine-drenched
73000: proto-anatolian, invoice-batch, phu-tsering, yuan-source, oc-eo, life-continuum, cost-prices, non-decline, chaos-bolts, anderson-sama
74000: lever-operated, boy-short, coal-box, may-britt, reality-bending, relaxed-looking, jak-stat, middle-ages, shadow-thing, all-destructive
75000: watch-guard, sea-weary, anti-long, eight-eight, anti-modernity, reflected-light, cortico-basal, k-state, counter-threat, franco-indian
76000: multi-island, six-saddle, luncheon-basket, whip-finish, non-demonstrative, non-infringing, profumo-farmaceutica, matter-wave, self-detachment, free-exercise
77000: half-shrugged, part-exchange, eighty-man, joint-service, anti-federalism, heat-seeker, dump-file, firm-jawed, honey-skinned, near-genocidal
78000: al-suhrawardi, minority-language, church-controlled, suite-like, electric-lit, pulse-rifles, steam-fried, ferrari-carano, mystery-religions, jtf-gtmo
79000: distance-viewer, note-passing, meadow-rue, ju-ching, stag-headed, non-question, mock-meat, white-bark, state-orchestrated, al-milal
80000: silver-mine, de-growth, meaner-looking, spoken-language, summer-vacation, cross-back, sweet-fern, flag-stoned, two-question, non-delinquent
81000: chat-huant, govett-brewster, hsien-sheng, jay-gardoqui, cross-default, shin-kobe, big-oh, konoe-san, chin-hua, albert-matesz
82000: five-storeyed, most-favoured, light-metal, parcel-gilt, hand-building, office-block, rickshaw-puller, de-nazified, pre-tv, top-price
83000: lost-world, motorcycle-taxi, tear-dampened, whim-whams, idol-worshiping, doggy-paddle, adult-adult, spot-cleaning, non-wetting, gordon-smith
84000: quasi-rents, risk-avoidance, wine-seller, radio-phonograph, metacarpo-phalangeal, seed-syllable, half-bewildered, ssri-induced, acid-secreting, waist-up
85000: in-bounds, front-leg, pesticide-contaminated, backward-sloping, caldwell-luc, carrier-bags, sway-back, just-fried, rust-hued, lower-gi
86000: dredged-up, mission-house, nicey-nicey, rim-rock, most-traveled, yin-fei, sta-prest, pre-life, wide-neck, radio-listening
87000: mark-downs, wheat-like, veggie-rich, sea-beaten, pressed-metal, v-src, crest-derived, merchant-princes, world-directed, heart-minds
88000: fire-eyes, semi-terrestrial, syringe-like, still-shaky, rum-infused, pkg-config, lie-up, charter-school, ratatouille-like, owl-headed
89000: dual-arm, dusty-blue, movie-studio, dual-power, beak-head, five-borough, picture-based, tower-houses, public-oriented, quasi-regular
90000: bond-type, sight-in, ajudou-a, ziggurat-like, chasseguet-smirgel, politico-moral, grey-box, pampas-grass, auditory-motor, food-taster
91000: title-deed, delegate-general, lion-taming, town-square, cypress-shaded, cursor-based, passport-control, al-yaqoubi, m-phase, no-par
92000: ex-leper, natural-color, ball-striking, body-work, two-bin, yellow-hat, non-tibetan, father-inlaw, honey-laden, non-supervisory
93000: mini-narrative, cool-mist, faux-fuyants, doutez-vous, love-relationship, gasp-worthy, davies-black, objective-subjective, paulsen-fuchs, low-diversity
94000: v-zone, laugh-track, pen-stroke, non-genuine, british-mandated, cybe-hibe, sleep-breathing, low-bid, pre-reagan, charge-hand
95000: semi-dome, psych-out, amber-haired, low-bellied, higher-echelon, e-booking, black-beaded, blossom-scented, paella-style, demi-culverins
96000: non-assistance, square-cube, famine-ravaged, fire-code, deeper-level, non-medically, sombre-faced, uv-light, more-substantial, bold-tasting
97000: neo-positivism, identity-politics, video-call, fernandez-armesto, gift-box, elbow-joint, horse-tails, progesterone-like, tomato-rich, nerve-stretching
98000: landing-parties, bug-house, back-fire, lo-ove, tobacco-spitting, not-eating, helsinki-vantaa, iron-wrought, villiers-stuart, half-sobbed
99000: monday-evening, m-rated, radial-engined, ready-grown, thick-thighed, silver-embossed, blue-overalled, chlorine-scented, alcohol-drenched, twin-propeller
```
