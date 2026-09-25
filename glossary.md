# HCCDA-AI — Glossary of Terms

Definitions with the boundary that separates each term from its neighbour, a concrete example (mostly the running textile-mill face-recognition case), the use case, and the developer-side form where it applies. Where Huawei uses a brand name that only _looks_ like an acronym, that is flagged.

---

**Adversarial fashion** — real-world attempts to fool computer-vision models: Kate Rose's license-plate shirts, CV Dazzle face paint (Adam Harvey), and the KU Leuven printed patch that defeats a person detector.

**Adversarial training / detection** — detection spots inputs deliberately crafted to fool a model; adversarial training retrains the model _on_ those attack examples so it stops being fooled. (Security–Enhance.)

**AI Cortex** — Huawei's industry-enablement platform: packages AI into industry solution blueprints/templates. Developer gets a solution blueprint + project templates. Use: a power utility builds grid AI from templates.

**AI Fuzzer** — bombards a model with malformed/random inputs to find where it breaks. (Security–Assess.)

**AI Gallery** — the marketplace ("app store") inside EI: publish/subscribe algorithms, models, datasets, workflows, notebooks; bridges publisher ↔ subscriber. It makes the ModelArts "Market" stage real. (EI as a whole = the mall; AI Gallery = its app store.)

**AI4S (AI for Science)** — AI applied to scientific discovery; flagship example AlphaFold.

**AIGC (AI-Generated Content)** — the Chinese/Huawei term for generative AI.

**All-domain sensing engine** — the "perceive" engine: vision, speech, OCR. Developer gets a callable API / deployable model. Use: the factory reads faces / detects a missing helmet. (The concrete, examinable engine.)

**Application enablement** — the stack layer that turns a trained model into a usable product/API (ModelArts, EI services). Job: deploy & consume. (vs frameworks = build; vs CANN = drive the chip.)

**Ascend** — Huawei's AI-accelerator chip family, built for matrix arithmetic. 910 = data-centre training; 310 = edge inference. Accelerates AI math (vs Kunpeng = general compute).

**ASR (Automatic Speech Recognition)** — speech → text (the perception direction). Half of SIS; the opposite of TTS. ASRC = ASR Customization (domain adaptation).

**Auto-labelling** — semi-supervised: you label a small batch, ModelArts predicts labels for the rest, and you verify. For face recognition the classes are the _people_, so it needs repeat photos of the same faces (recurrence) to propagate identities; it compares face embeddings (close vectors = same person).

**Batch service** — see _Real-time service_. Asynchronous: process a whole OBS folder of inputs offline, write the results back, then finish. (Forms left overnight, vs a shopkeeper at the counter.)

**Behaviorism** — the school that holds intelligence = feedback from acting; its computational form is reinforcement learning. Learns behaviors no one can specify, but needs trial-and-error and a well-designed reward. (One of three schools now converging with symbolism and connectionism.)

**Build vs consume** — FRS/SIS/OCR are pretrained services you _call_ (consume); ExeML is where you _build your own_ model. The full-stack view is build; the Cloud-EI view is consume.

**CANN (Compute Architecture for Neural Networks)** — the chip-enablement layer between the framework and Ascend silicon; turns high-level operations into chip instructions. Huawei's equivalent of CUDA. Job: make the hardware usable.

**Capsule networks (Hinton)** — units output vectors encoding a part's pose, combined by "routing by agreement." Same goal as the transformer (encode relationships between parts) but iterative/slow and never scaled. Attention is routing-by-agreement that trains efficiently — "the architecture that fits the hardware wins."

**CNN (Convolutional Neural Network)** — slides small learnable filters over a grid to detect spatial patterns; for **space** (images) — face recognition, medical scans, object detection. Really a dense layer with two constraints: weight sharing + locality (the built-in prior "this input is an image"). Mainstream 1998 (LeNet) / 2012 (AlexNet).

**Concept drift detection** — flags when live data has drifted from the training data, so accuracy silently drops. (Other–Assess.)

**Connectionism** — the school that holds intelligence = learning from data via neural networks; the deep-learning school that won. Handles messy, high-dimensional data but is a black box (SHAP buys back some transparency).

**Convergence of learning modes (enhanced intelligent perception)** — no single mode suffices, so several are stacked, each covering another's weakness. Factory camera: transfer + supervised + multimodal + reinforcement + knowledge computing cooperating on one event. The market has moved from single-model demos to fused pipelines. Real products: ChatGPT (five modes), Tesla FSD, TikTok/YouTube recommendations, DALL·E/Midjourney, AlphaGo/AlphaFold, Google Search.

**CPU vs GPU vs Ascend** — CPU = a few powerful cores (sequential work); GPU = thousands of simple identical cores (the same operation on masses of data); Ascend = narrower still, matrix multiply-accumulate. A ModelArts training job runs on one of these — a cost/speed choice.

**DataArts** — data enablement: the toolkit that cleans, organizes, and pipelines data (the prep station that "washes, chops, and organizes the ingredients").

**Data lake** — a store of raw data of ANY kind, including unstructured (images, audio, text, logs); schema-on-read; feeds AI. The biggest and rawest of the three stores.

**Data mart** — one department's slice of a data warehouse; same structure, narrower scope; the smallest of the three.

**Data warehouse** — a store of cleaned, structured data in tables; schema-on-write; for analysis and reporting. A photo cannot live here.

**Decision optimization engine (OptVerse)** — the "decide" engine: given a goal and constraints, find the best action. Developer gets a solver invoked as a service. Use: the optimal production schedule.

**Deep (deep learning)** — many stacked hidden layers, each abstracting the one below (edges → parts → identity). One hidden layer = _shallow_, by definition. Universal approximation: one wide layer can approximate almost anything in principle, but depth does it efficiently and builds a feature hierarchy. No fixed threshold; more than one hidden layer = deep, conventionally.

**Deploy as a real-time service** — publish a trained model as a live, always-on HTTPS API that answers one request synchronously in milliseconds; billed by the second until stopped. "Switch the brain on and give it a phone number." Stopping the service is a graded exam step (billing hygiene).

**Differential privacy training** — add _calibrated_ noise (a tuned amount, not random randomness) to the gradients during training, so no single person is recoverable but the overall pattern survives. The dial is ε (the privacy budget): small ε = more noise, stronger privacy, lower accuracy. Guarantee: the model behaves almost the same whether or not any one person was in the set. A trade-off, not a free lunch (worse on small datasets like 800 workers).

**Diffusion** — the "painter": trained to reverse added noise, so it generates an image from pure noise.

**DIKIW** — Data →(labeling)→ Information →(associating)→ Knowledge →(mining)→ Insight →(judgment)→ Wisdom. Value, processing, and personalization rise together; Knowledge → Insight is the key jump (shared rules → your personal baseline). Wearable: LED light ticks → HR 72 bpm → "high HR + low motion is unusual" → your resting HR drifting up → "rest today."

**Downstream task** — the specific job a pretrained model is pointed at.

**Edge device** — a gateway one hop up from end devices, aggregating several of them (the Atlas box in the mill). See _End device_.

**EI (Enterprise Intelligence)** — Huawei Cloud's umbrella brand for its business-facing AI services (vision, NLP, speech, OCR, Pangu, ModelArts-as-a-service).

**Elements of AI** — data, algorithms, computing power, and scenarios. Frameworks are removable tooling (their own layer), not an element.

**End device** — the origin of data: a camera, sensor, or phone. (vs edge device = the gateway aggregating several end devices.)

**ExeML (Exclusive / Executable ML)** — codeless AutoML inside ModelArts: label → train → deploy, no code. Four project types: image classification, object detection, sound classification, and predictive analytics (tabular). "Exclusive" = a model exclusive to _you_ (your data, your task) vs the shared services. Not an official acronym — teach it as a brand for "code-free automated ML."

**Fault tolerance metrics** — measure how gracefully a model handles errors and hardware faults. (Other–Assess.)

**Few-shot learning** — instruction + a few worked examples in the prompt ("shot" = example). Changes no weights (in-context learning). vs zero-shot (instruction only) and fine-tuning (a new checkpoint).

**Fine-tuning** — further training that produces a new, modified checkpoint. Contrast zero-/few-shot, which leave the weights unchanged.

**Frameworks** — what you build models in (MindSpore, TensorFlow, PyTorch, Caffe): prebuilt layers, autodiff, training loops. They sit on CANN. Job: make model-building practical.

**FRS (Face Recognition Service)** — Huawei's pretrained EI vision service for face recognition.

**Full stack vs Cloud EI** — the same architecture from two angles: the full stack is the vertical engineering view (build); Cloud EI is that same stack sold as a catalogue of services (consume). "EI is the full stack with a shopping cart." "All-scenario" = the stack spans device / edge / cloud.

**FusionInsight** — Huawei's big-data platform: the data lake itself (storage/reservoir, on Hadoop/Spark/GaussDB). Flow: FusionInsight (source) → DataArts (prep) → ModelArts (train). The lake is upstream; DataArts is the prep layer on top of it.

**GAN (Generative Adversarial Network)** — two networks compete, a generator making fakes and a discriminator catching them, until the fakes convince. Examples: ThisPersonDoesNotExist, face-swap filters.

**Gartner Hype Cycle** — trigger → peak of inflated expectations → trough of disillusionment → slope of enlightenment → plateau of productivity. The trough is where a technology loses its marketing and finds its real job.

**HiAI** — Huawei's on-device AI platform that lets apps tap the phone's NPU (Kirin). Layers: Foundation (chip) / Engine (app) / Service. Device/NPU-facing.

**HiLens** — Huawei's edge-vision platform: prebuilt camera/vision "skills" + a cloud console to deploy them to Ascend devices; rides on IEF underneath. Reach for it first on a vision job; drop to IEF for non-vision work (e.g. loom sensors). A brand name, not an official acronym ("Hi" = Huawei's AI prefix, "Lens" = camera).

**HMS ML Kit (Huawei Mobile Services ML Kit)** — an app-developer SDK of drop-in on-device AI features (text/face/image recognition, translation). App-facing.

**Hybrid cloud** — your own on-premises setup + rented public cloud, working as one, so private data stays home while the cloud does the heavy lifting. The factory edge box (local) + ModelArts (cloud) is a hybrid. Note: "heavy training in the cloud" and "data never leaves" only coexist by decoupling — pretrain the generic part on public data then fine-tune locally, use federated learning, or encrypt/TEE; plain hybrid means the data goes up under guard.

**IEF (Intelligent EdgeFabric)** — a cloud service that _manages_ edge devices: pushes models down, pulls alerts and health up. Lightweight — "remote control for edge boxes." Only management traffic travels; the data stays at the edge. Factory: delivers the face model to the mill's box and tells you at 3 a.m. that box #7 is offline.

**IES (Intelligent EdgeSite)** — heavyweight: Huawei parks a full managed cloud rack _in your building_ ("a mini Huawei Cloud in your basement"). Usually rented/managed (Huawei owns the on-site rack; you pay cloud-style), not bought — buying hardware outright is the separate Atlas/edge route.

**IP cores** — the "IP" in the stack = intellectual-property blocks (e.g. the Da Vinci architecture), not "intelligent processor."

**Knowledge computing** — (a learning mode and an engine) build a knowledge graph of entities and relationships, then reason over it. Examples: Google's knowledge panel; Huawei's industry graphs. As an engine it is the "reason" one — developer gets a graph service + query API; use: diagnose a fault from a symptom.

**Kunpeng** — Huawei's ARM-based general-purpose server CPU (an Intel/AMD alternative): runs the OS, databases, data prep. A partner to Ascend, not a rival — a ModelArts job uses both (Kunpeng orchestrates, Ascend crunches).

**L0 / L1 / L2 models (Pangu tiers)** — L0 = foundation (general; one per domain: NLP / CV / multimodal / prediction / scientific); L1 = industry (L0 + sector data); L2 = scenario (fine-tuned for one task). "L0 general → L1 your industry → L2 your exact job" — transfer learning down a hierarchy. The Pangu "siloed vs industrial" slide is the L0 → L2 story (L1 implied).

**Market stage (ModelArts)** — the final lifecycle stage where the built model joins a shared marketplace: reused internally (across the enterprise) + externally (partners, third parties) = the AI Gallery. "Now let others use it," not a separate product.

**MindX** — Huawei's enterprise AI-enablement suite on Ascend (MindX DL / Edge / Foundation + industry SKUs). Enterprise/Ascend-facing.

**Model (artifact)** — a checkpoint file of learned numbers plus an architecture description; the training data does not ship with it.

**ModelArts** — Huawei's one-stop AI development platform (data prep, training, deployment, lifecycle). It is the overlap of frameworks + application enablement: app-enablement that runs frameworks inside it. Pure framework = MindSpore on a bare machine; pure enablement = calling an FRS/SIS API; the overlap = a ModelArts notebook running MindSpore.

**ModelArts access tiers** — three doors ranked by expertise: ExeML (codeless / AutoML) → built-in algorithms (beginner) → notebooks + frameworks (engineer).

**ModelArts editions** — Lite (bring-your-own-resources, minimal) / Edge (deploy to edge devices) / Standard (full managed public cloud — the default students use) / Alliance (cross-organisation, multi-party, federated). Exact boundaries shift in the docs.

**ModelArts lifecycle (5 stages)** — Data → AI data framework (filter, auto-label) → Algorithm development (frameworks, MoXing, built-in models) → Training (wizard ExeML, distributed cluster, weeks → minutes) → Deployment (device-edge-cloud, Ascend inference) → Market. A grey "workflow versioning / traceable" band runs under all five.

**Model protection: encryption vs obfuscation vs fingerprint** — encryption locks the file (it can't run without the key); obfuscation lets it _run_ but makes the internals unreadable (shuffled weights, disguised values, a rewritten graph — raise-the-cost, not impossible); fingerprint hides a signature so you can _prove_ a stolen model is yours.

**MoXing** — ModelArts' high-level deep-learning library (wraps TF / MindSpore / PyTorch); handles distributed multi-chip training and OBS read/write automatically. "Moxing" = 模型 (model). The glue for "read from OBS, train at scale."

**Multi-architecture compute** — the processors ModelArts editions can run on: CPU, Kunpeng (general) + GPU pool, Ascend pool (AI acceleration).

**Multimodality / Multimodal learning** — one model working across text, image, and audio together (GPT-4/5, Gemini, Pangu). See also _Same token space_.

**OBS (Object Storage Service)** — Huawei's cloud storage (= AWS S3). You park data in a _bucket_; ModelArts reads its datasets from OBS, not from local disk.

**OCR (Optical Character Recognition)** — text extraction from images/documents. See also _Table OCR_.

**Optimization** — in "big data + algorithms + optimization," the training-time machinery: gradient descent/Adam, learning rate, schedules. A design choice that disappears after training and never ships in the model file.

**OptVerse AI Solver** — Huawei's ML-guided optimization solver. Solves a _fully specified_ problem (variables, constraints, objective) and computes the answer (a 3-day plan in 1 minute). Not "Opus." (vs RL engines, which learn in an environment you can't fully specify.)

**Pangu Models** — Huawei's foundation models (L0), for industrial-scale AI across CV / NLP / multimodal / prediction / scientific computing.

**Parameters** — the learned numbers = weights + biases. A 768 → 768 layer has 768 × 768 ≈ 590k weights.

**PCL-α** — Peng Cheng Lab's ~100B-parameter Chinese LLM built with Huawei (not "PCL-L"); sibling of Pangu-α (PanGu-Σ ≈ 1.085T).

**Privacy-protecting training** — keep raw data where it lives and let only the learning travel: federated learning (the model goes to the device, only updates return, the cloud averages — the device-edge-cloud pattern as a privacy method), homomorphic encryption (compute on encrypted data), MPC (parties exchange scrambled shares), TEE (train inside a sealed hardware enclave). Gold standard = federated + differential privacy together. (Contrast: DP sees the data but hides the individual; federated never moves the data; encryption computes on unreadable data.)

**Prompting** — the "steering wheel": an interface to a frozen model, not an architecture.

**Public cloud** — everything rented and off-site, on Huawei's infrastructure. (vs hybrid = on-prem + rented together.)

**Real-time service** — synchronous, one request answered now (a shopkeeper at the counter). vs batch (asynchronous, a whole folder offline) and edge (pushed to a device via IEF). See _Deploy as a real-time service_.

**Reinforcement learning (RL)** — an agent acts, gets a reward or penalty, and improves by trial and error (the behaviorism school). Examples: AlphaGo; the RLHF step that made ChatGPT helpful.

**ResNet (Deep Residual Network)** — the same thing under a long name and its nickname. Each block learns a _residual_ (a small change) via a skip connection that also gives the gradient a shortcut, enabling networks 50–1000 layers deep. It is an architecture (a published blueprint), NOT a product: companies sell face recognition / medical diagnosis _built on_ it. The default vision backbone (increasingly alongside ViT).

**RL engines** — Huawei's reinforcement-learning toolkits for decision-making ("learn the best action by trial and reward"). vs OptVerse: RL learns in an environment you can't fully specify (robot control, live-system tuning); OptVerse computes a written-down problem. A portfolio category, not one SKU.

**RNN (Recurrent Neural Network)** — a network with a loop that carries state across a sequence; for **time** (speech, text, time series), now largely replaced by transformers. Came first (LSTM, 1997). Mnemonic: CNN = space, RNN = time.

**Robot engine** — the "act" engine: perception + control for machines. Developer gets an SDK + edge-deployed model. Use: a warehouse arm that picks and sorts parcels.

**Robustness (Hoeffding's inequality)** — a statistical bound estimating how stable a model's accuracy is, giving a confidence guarantee on robustness. (Security–Assess.)

**Same token space** — text, image, and audio are all converted into same-shape vectors in one shared coordinate system, so the model treats a word, a patch, and a sound as the same kind of object and can reason across them. The old way used a separate model per modality. ("Tokens are all you need.")

**Scenario** (Huawei term) — the concrete business problem that supplies the objective, the data, and the constraints (factory defect detection, fraud detection, smart traffic). "All-scenario" = the same stack across device / edge / cloud. Huawei organizes its pitch around scenarios because it sells industry solutions.

**SIS (Speech Interaction Service)** — the EI service bundling ASR (speech → text) and TTS (text → speech): two opposite arrows in one service.

**Stable Diffusion** — text-to-image generation via denoising (a diffusion model).

**Supervised learning** — learn from labelled examples (e.g. the Gmail spam filter learning from the emails you marked spam).

**Symbolism** — the school that holds intelligence = logic and explicit rules (expert systems, knowledge graphs, Deep Blue). Transparent, but every rule is hand-written. Also called logicist.

**Table OCR** — OCR that also recovers the table's structure (which word is in which cell, and the row/column layout), returning structured output so a scanned table rebuilds as a spreadsheet. Plain OCR loses the grid.

**Tokens** — integer IDs from a vocabulary of word-pieces (~4 characters of English each); each indexes an embedding table to become a vector (e.g. 768 floats). Image patches and audio slices become vectors the same way.

**Transfer learning** — repurpose a pretrained model for your small task instead of training from scratch. Examples: the factory FRS fine-tune; medical imaging on an ImageNet backbone (the sharpest example — nobody has millions of labelled scans); every fine-tuned LLM; Whisper adapted to Urdu.

**Transformer** — the "skeleton." Attention: each element's vector becomes a query, a key, and a value; A attends to B via a query·key dot product, softmaxed, then a weighted sum of values — one big matrix multiply (parallel). An "element" is whatever you chopped the input into (text token, image patch, audio slice), which is why the same architecture handles every modality.

**Trustworthy AI (Assess × Enhance)** — a grid: columns Security / Privacy / Other × rows Assess (measure a weakness) / Enhance (fix it). The top row asks "how vulnerable is my model?"; the bottom row does something about it.

**TTS (Text-to-Speech)** — text → audio (the generation direction). Half of SIS; the opposite of ASR.

**VAE (Variational Autoencoder)** — an encoder/decoder that encodes to a _distribution_ rather than a point, giving a smooth, samplable latent space; blurrier than GAN/diffusion, but its latent-space idea lives on inside diffusion models.

**ViT (Vision Transformer)** — slices an image into patches, adds positional encoding, and uses attention to find patch relationships. Needs far more data than a CNN (it assumes nothing spatial), so production face recognition often still uses a CNN; the edge box probably runs a CNN.

**Zero-shot learning** — instruction only, no examples; the model answers from pretraining. vs few-shot (a few examples) and fine-tuning (new weights).

**Zettabyte** — a trillion gigabytes.
