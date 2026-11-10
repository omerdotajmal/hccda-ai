# HCCDA-AI — Glossary of Terms, Part 2

Terms added since the first glossary. This part covers cloud fundamentals and access, the computer vision task taxonomy and its evaluation metrics, image classification in depth, the nine CV challenges, the NLP stack, speech interaction, requirement analysis, and the fever-screening case study.

Each entry gives the definition, the boundary that separates the term from the one it is usually confused with, and a concrete example. Terms from the first glossary are not repeated here.

**Jump to:** [A](#a) · [B](#b) · [C](#c) · [D](#d) · [E](#e) · [F](#f) · [H](#h) · [I](#i) · [K](#k) · [L](#l) · [M](#m) · [N](#n) · [O](#o) · [P](#p) · [R](#r) · [S](#s) · [T](#t) · [V](#v) · [W](#w) · [Appendix](#appendix--which-metric-goes-with-which-task)

---

## A

**Accuracy** — the share of all predictions that are correct. Misleading on imbalanced data: if 1% of patients have a fever, a model that always answers "no fever" is 99% accurate and completely useless. Report it alongside precision and recall, never alone.

**AK/SK** (Access Key ID / Secret Access Key) — the permanent credential pair for *programmatic* access to Huawei Cloud: SDKs, REST calls, the CLI, OBS Browser. The AK (~20 characters) is the public identifier naming who is calling, and it travels openly in the request headers. The SK (~40 characters) is secret and is never transmitted: the SDK uses it as the key to compute an HMAC signature over the request (method, URI, headers, timestamp, body) and sends only that signature. The server looks up the SK by the AK, recomputes the signature and compares — a match proves you hold the secret without the secret ever crossing the wire, and the signed timestamp stops a captured request being replayed later. Handling: created under *My Credentials* / IAM and downloadable exactly once as `credentials.csv`; there is no way to view an SK again, so a lost SK means deleting the pair and creating a new one (limit of two per IAM user). A leaked pair grants everything that IAM user may do, so it belongs in an environment variable or an untracked config file — never hardcoded in a notebook that will be shared or pushed. Exam note: console sign-in → password; API or SDK → AK/SK. See also **Token**.

**AZ** (Availability Zone) — a physically separate data centre inside a Region, with its own power, cooling and network feed, linked to its sibling AZs by private low-latency fibre (about 1–2 ms). The AZ is the *failure domain*: a fire, flood or power cut takes out one AZ while the others keep running. Boundary vs Region: the Region is where in the world, the AZ is which building there. A region typically has two or more, named AZ1, AZ2, AZ3. AZ selection is an IaaS-level choice — you pick one when creating an ECS or an EVS disk, while managed regional services hide it (an OBS bucket is created for a region and is already replicated across AZs, so there is no AZ field to set). Rule of thumb: if you chose the machine, you chose the AZ.

---

## B

**Backbone and classification head** — the backbone is the pre-trained, feature-extracting part of a network (ResNet, MobileNet, ViT); the head is the small final layer or two that maps those features to *your* classes, ending in softmax or sigmoid. Transfer learning for classification means keeping the backbone, replacing the head, and fine-tuning on a few hundred images per class. ExeML automates exactly this route.

**Background interference** (camouflage) — a CV challenge: the object blends into a background of similar colour or texture, so its edges disappear. Examples: tiger stripes in tall grass, brown leaf spots on brown soil, a fish over cobblestones. Fix: many different backgrounds in the training data, or a plain background where the setup allows it.

**Background noise** — a speech-interaction challenge covering both ambient sound (bazaar, traffic, fans, other voices) *and* microphone and transport-channel quality. Exam trap: microphone quality belongs here, not under input variation. Fix: train on noisy recordings, apply noise reduction, and test on real phone calls rather than studio audio.

**Behavior recognition** (action or activity recognition) — advanced vision: deciding what a person, vehicle or animal is *doing* across a clip — walking, falling, fighting, smoking, using a phone. It needs time: a single frame of a raised arm cannot distinguish waving from hailing a rickshaw. Boundary vs pose estimation, which gives body keypoints in one frame and is often the *input* to behaviour recognition; boundary vs tracking, which answers where something goes rather than what it does. Examples: fall detection in elderly care, dangerous-behaviour checks in a coal mine, exam-hall alerts. Developer side: 3D CNNs (C3D, I3D), SlowFast, skeleton-based ST-GCN, video transformers; runs at the edge on HiLens.

**Binary, multi-class and multi-label classification** — binary: two classes (defect / no defect). Multi-class: exactly one of many mutually exclusive classes, with scores from **softmax** that sum to 1. Multi-label: several labels may be true at once, each with its own independent score from a **sigmoid**, so the scores need not sum to 1. A photo tagged "Motorway 100%, Road 90%, Highway 63%, Sky 55%" is multi-label; Huawei's Image Tagging API is multi-label.

**Blackbody calibrator** (blackbody reference) — a small device whose surface is held at a precisely known temperature (say 35.0 °C) and kept permanently in the thermal camera's view. Thermal cameras drift with ambient temperature and age, so comparing each frame's reading of the blackbody against its known value lets the system correct that drift — which is how a ±0.3 °C accuracy target becomes achievable. Set to fever-level temperatures it also provides safe test readings without needing sick people. Boundary vs image alignment: alignment fixes *where* the face is in the thermal image, the blackbody fixes *how accurate* the temperature reading is.

**BLEU** — the standard metric for machine translation quality: it scores how much the machine's output overlaps with one or more human reference translations.

**Blocked objects** (occlusion) — a CV challenge: only part of the object is visible because something stands in front of it, such as pedestrians behind rickshaws in a bazaar. Fix: include partly hidden examples in the training data, raise the camera, or add a second angle.

---

## C

**Character precision rate / character recall rate** (CPR / CRR) — the OCR metrics. CPR: of the characters the system returned, the share that are correct. CRR: of the characters actually present on the page, the share the system found. A system that returns very little can score high precision and poor recall, so both are quoted together.

**Classification algorithm lineage** — four generations worth recognising: (1) classical, pre-2012 — hand-crafted features (colour histograms, HOG, SIFT) fed to a classical classifier such as SVM, k-NN or random forest; (2) CNNs — AlexNet (2012), VGG and GoogLeNet/Inception (2014), ResNet (2015), then light models for phones and edge devices (MobileNet 2017, EfficientNet 2019); (3) Vision Transformer (ViT, 2020); (4) foundation models — CLIP (2021), which classifies zero-shot by matching an image against class names written as text, and Pangu CV models.

**Closed-set limitation** — a classifier can only answer with a class it was trained on. Show it something outside that set and it will force the image into the nearest known class, often with high confidence. Fixes: add an explicit "other / unknown" class, or set a confidence threshold that routes uncertain images to a human. Related wording in testing: "samples of unknown classes", as opposed to "hard samples", which are known classes the model simply gets wrong.

**Code-switching** — mixing two languages in one sentence ("balance check karo"). Extremely common in Pakistani speech and text, and difficult for models trained on a single language.

**Computer vision — the three levels** — one framework, seen two ways. **Level 1, foundation / primary vision / image processing**: works on pixels; an image goes in and a better image comes out; it knows nothing about content. Tasks: super-resolution, inpainting. **Level 2, middle level / mid-level vision / image analysis**: works on edges, direction and texture, grouping pixels into regions and objects; the output is what is where and what shape — labels, boxes, masks. Tasks: object detection, image segmentation, image classification. **Level 3, advanced vision / image understanding**: works on named objects and the scene as a whole — relationships, language, time; the output is sentences, answers, similar images, trajectories, activities. Tasks: image description, image Q&A, image search, visual tracking, behavior recognition. The unifying rule: mid-level finds and labels *individual things*, advanced understands *the scene as a whole*. A CNN climbs the same three levels internally — early layers learn edge and orientation detectors, middle layers textures and parts, deep layers whole objects. Exam note: the two name sets are interchangeable by position, so "what does the middle level work on?" → edge, direction, texture, and "which level is detection?" → mid-level.

**Computer vision challenges** (the nine) — the organising idea is that the object did not change, the pixels did. Three families: **light and camera** — illumination change, motion blur, low resolution; **geometry** — scale variation, posture variation, in-plane rotation, hyperplane rotation; **surroundings** — blocked objects, background interference. Each challenge becomes one robustness requirement and one test feature factor. Fixes come in two kinds: data fixes (collect varied photos, augment) and camera or setup fixes (better hardware, better placement) — the second kind is a hardware requirement, not a model fix.

**Confusion matrix** — a table of true class against predicted class. The diagonal holds the correct predictions; the off-diagonal cells show exactly which classes are being mixed up, which is what tells you whether to collect more data or merge two classes.

**Conversation assistant** — an NLP application: a chatbot or voice assistant that holds a multi-turn conversation with a user, as opposed to answering one isolated question.

---

## D

**Data augmentation** — creating extra training variety by randomly flipping, rotating, cropping, changing the brightness of, or adding noise to the images you already have. A cheap defence against illumination, rotation and scale challenges, and against overfitting on a small dataset. Note what it cannot fix: rotating a 2-D photo never reveals a hidden side, so it does nothing for hyperplane rotation.

**Data drift** — the images or inputs arriving in production gradually diverge from the training data over time (new phone cameras, a changed production line, a new season), and accuracy silently falls. Boundary vs domain shift: domain shift is the mismatch being there from day one; drift is it appearing later.

**Dialog management** — the step in the voice-interaction pipeline (speech recognition → language understanding → **dialog management** → language generation → text-to-speech) that tracks the conversation so far and decides what the system does next: remember what has been said, ask for a missing detail, confirm before acting, carry out the action. Example: a caller says "card band karo"; the dialog manager notes the intent, sees that the caller is not yet verified, asks a security question, then confirms "Block your card ending 4421?" before acting. Boundary vs language understanding: understanding works out what *this one sentence* means; dialog management decides what to do *given the whole conversation*.

**Direction** (orientation) — one of the mid-level image features: which way edges and patterns run — horizontal, vertical, diagonal — computed from the brightness gradient. Stripes run across a tiger's body while grass blades point upward, and that difference alone helps separate the two regions. Developer side: HOG, Gabor filters; a CNN's first-layer filters learn orientation detectors by themselves.

**DLI** (Data Lake Insight) — Huawei Cloud's serverless big-data analytics service (Spark, Flink, SQL) for processing very large datasets without managing a cluster. In the NLP stack it is what crunches large text corpora. Boundary: OBS stores files, RDS stores rows you query, DLI analyses data at scale.

**Domain shift** — the images the model meets in use differ systematically from the ones it trained on: clean lab photos versus farmers' phone photos, daytime versus night, staged footage versus real behaviour. It is the main reason a model that scored well in testing disappoints in the field.

---

## E

**ECS** (Elastic Cloud Server) — Huawei Cloud's virtual machine: a rented computer you hold root or admin on, assembled from CPU, memory, an OS image and EVS disks. Once created you use it like your own PC — install what you like, run services, open ports. "Elastic" means the specification is not fixed at purchase: resize vCPU and RAM, attach more disk, add or remove servers as load changes, and pay by the hour or month. It is the IaaS tier — Huawei runs the hardware and hypervisor, you run everything from the OS upward. Equivalent to AWS EC2 or Azure VM. Boundary vs ModelArts, which is PaaS: with ModelArts you submit a training job and never see the machine, while an ECS hands you both the control and the maintenance (patching, scaling and uptime are yours). Rule of thumb: a *custom application* that calls AI APIs — a Flask app wrapping an OCR endpoint, a scheduler polling OBS — goes on an ECS; training or deploying a *model* goes to ModelArts. Boundary vs OBS: ECS computes, OBS stores; a bucket has no CPU, so nothing runs "inside" it. Exam note: ECS is general compute that happens to be in the cloud, not an AI service.

**Edge** (image feature) — one of the mid-level features: a place where brightness or colour changes sharply, which is usually an object's boundary, such as a tiger's outline against grass. Grouping pixels of similar texture inside connected edges is what splits an image into regions. Developer side: Sobel and Canny edge detectors — Canny reappears later as a ControlNet condition.

**EVS** (Elastic Volume Service) — the block storage you attach to an ECS. It behaves like a physical hard disk: it gets a drive letter or mount point, is formatted with a filesystem, and is readable only by the server it is attached to. Boundary vs OBS: EVS is block storage for one machine (a disk), OBS is object storage reachable by anything holding credentials (a shared drop-box). An EVS disk attaches only to an ECS in its own AZ. Example: a 100 GB EVS disk holding a database's files on an ECS, while the nightly backup is pushed to an OBS bucket.

---

## F

**F1 score** — the harmonic mean of precision and recall, used when you need a single number and both false alarms and misses matter. The harmonic mean punishes imbalance: a model with precision 1.0 and recall 0.1 scores about 0.18, not 0.55.

---

## H

**HOG** (histogram of oriented gradients) — a classical hand-crafted feature: divide the image into small cells, and for each cell record how much edge energy runs in each direction. It describes a whole region's gradient directions, which is the boundary against SIFT, which describes individual keypoints. The workhorse of pedestrian detection before deep learning.

**Hyperplane rotation** (the standard term is out-of-plane rotation) — a CV challenge: the object turns toward or away from the camera, spinning around an axis that lies in the plane of the photo — X (a nod, pitch) or Y (a shake, yaw) — so some parts disappear and new sides appear. A book turning like a door shows its spine; a face turning sideways goes front → three-quarter → profile, losing an eye and gaining an ear. Boundary vs in-plane rotation: augmentation cannot fix this one, because rotating a flat photo never reveals the hidden side. You need real photographs taken from many angles.

---

## I

**Illumination change** — a CV challenge: over- or under-exposure makes the same object produce very different pixel values. Noon sun versus night CCTV, or glare from a shop doorway. Fix: day and night photos in the training set, brightness augmentation, or an infrared camera for night.

**Image alignment** (image registration) — mapping one image onto another so that the same point in the scene sits at the same pixel position in both. In fever screening, the face is found in the HD image but the temperature must be read from the thermal image; the two cameras sit side by side with different lenses, fields of view and resolutions, so the face box has to be shifted, scaled and slightly rotated into thermal-image coordinates. It is done once by calibration against a target visible to both cameras, then applied to every frame. OpenCV provides it, it aligns within about a second, and it runs on an ordinary PC. Analogy: two phones photographing the class side by side — to find the same student in both photos you shift and resize one onto the other.

**Image classification** — mid-level vision: one label, or a few, for the whole image. It says *what*, never *where* or *how many*. Input: one image resized to the fixed size the model expects (commonly 224×224×3) with normalised pixel values; training needs many images each tagged with its class, one folder or tag per class, stored in OBS for ModelArts. Output: a confidence score per class, the highest being the prediction. Boundary vs detection: a classifier says "this photo contains a helmet", a detector says "three helmets, here, here and here". Example: a cotton-leaf photo sorted into healthy / leaf curl / whitefly damage. Metrics: accuracy, precision, recall, F1, ROC and PR curves, confusion matrix. Huawei routes — consume: the Image Recognition service and its Image Tagging API (pre-trained, multi-label, no training needed); build: a ModelArts ExeML image classification project (zero code) or custom training in a notebook with PyTorch or MindSpore, deployed as a real-time service, a batch service, or to the edge via HiLens.

**Image description** (image captioning) — advanced vision: generates a natural-language sentence describing an image, covering objects plus their relationships and actions. Boundary vs tagging: tags are a list ("dog, grass, frisbee"), a caption relates them ("a dog leaps to catch a frisbee on a lawn"). Example: automatic alt-text so screen-reader users hear what a photo shows. Developer side: CNN encoder with an RNN decoder (Show and Tell, 2015), then BLIP, now multimodal LLMs; metrics BLEU and CIDEr.

**Image inpainting** — primary vision: fills a missing, damaged or deliberately masked region with content that blends into its surroundings. The input is the image plus a mask of the hole. Boundary vs super-resolution, which adds detail everywhere at a larger size, while inpainting fills one hole at the same size; boundary vs text-to-image, since inpainting edits an existing image. Examples: removing a fold or scratch from a scanned old photo, erasing a passer-by (Magic Eraser, Generative Fill). Developer side: OpenCV's `cv2.inpaint` (classical Telea and Navier–Stokes methods), LaMa, and diffusion models given a mask.

**Image Q&A** (Visual Question Answering, VQA) — advanced vision: answers a free-form question about an image. Input is image plus question, output is the answer. Boundary vs captioning: a caption describes whatever is most salient, unprompted, whereas VQA answers *your* question, which may require counting, reading text, or spatial reasoning. Example: a visually impaired user photographs a medicine strip and asks for the expiry date. Developer side: the VQA benchmark (2015), now multimodal LLMs — GPT-4o, Gemini, Qwen-VL, Pangu multimodal.

**Image search** (content-based image retrieval, search by image) — advanced vision: given a query image or text, return the most similar images from a large collection, ranked. Every image becomes a feature vector, and search is a nearest-neighbour lookup. Boundary vs classification: there is no fixed class list and no labels are needed, because similarity is open-ended; boundary vs keyword search, which matches filenames and tags rather than pixels. Examples: reverse image search, photographing a lawn print to find similar suits in an online store, detecting duplicate product listings. Developer side: CNN or CLIP embeddings with a vector index such as FAISS or Milvus; CLIP also enables text-to-image search.

**Image segmentation** — mid-level vision: labels every pixel, producing a mask rather than a box. Used when shape, area or an exact boundary matters. Boundary vs detection: a box is a rectangle that necessarily includes background, while a mask follows the true outline. Examples: the percentage of a leaf that is diseased, drivable road area, a tumour outline in a scan. Metric: MIoU. Developer side: U-Net and DeepLab (semantic), Mask R-CNN (instance), SAM. See **Semantic segmentation**, **Instance segmentation**, **Panoptic segmentation**.

**ImageNet** — the benchmark that drove modern computer vision: the ILSVRC subset holds about 1.28 million training images across 1,000 classes, and most pre-trained backbones are trained on it. AlexNet's 2012 win (15.3% top-5 error against 26.2% for the runner-up) began the deep-learning era; ResNet reached 3.57% in 2015, below the roughly 5% human estimate.

**In-plane rotation** — a CV challenge: the object rotates within the image plane, like a steering wheel — that is, around the camera's line of sight. Take the photo as the X–Y plane with Z pointing at the camera: in-plane rotation is a spin around Z, which for a head is *roll*, ear toward shoulder. Every part stays visible, as with a book held upside down or sideways. Fix: rotation augmentation, which works perfectly here because rotating the training image recreates the case exactly.

**Input variation** — a speech-interaction challenge: the same words sound different from different people and at different moments. Vocal-cord differences (child, elderly, hoarse), accents and speaking styles (Urdu carrying Saraiki, Punjabi or Pashto sounds, or code-switching with English), and emotion, time of day or physical condition. Fix: record many speakers across ages, regions and moods.

**Instance segmentation** — every individual countable object gets its own mask (car 1, car 2, car 3), so objects can be counted. Usually built on detection: find each object's box, then draw a mask inside it (Mask R-CNN); bottom-up methods instead split a semantic map into separate objects. Note the direction — instance builds on detection or on semantic segmentation, never the reverse. Classroom analogy: semantic segmentation paints every student "person" in one blue, instance segmentation gives each student a different colour.

**Intelligent outbound calls** — an NLP application: automated outgoing phone calls for reminders, follow-ups and confirmations, where the system both speaks and understands the reply.

**Intelligent Q&A** — an NLP application: answering user questions from a knowledge base or FAQ set, matching the question by meaning rather than by exact keyword.

**Intelligent speech quality check** — an NLP application used in call centres: every call is transcribed with ASR and then checked by NLP for script compliance, tone and required keywords. The point is coverage — a human supervisor can sample a handful of calls, while this checks all of them.

**IoU** (Intersection over Union) — the overlap between a predicted and a true region divided by their combined area. 1 means perfect overlap, 0 means none. A detection usually counts as correct only at IoU ≥ 0.5. It is the building block of both mAP and MIoU.

---

## K

**Keyword extraction** — picking out the terms that best summarise a document, used for tagging, indexing and routing.

---

## L

**Language generation** (LG / NLG) — meaning or input in, new text out: text generation, summarisation, text correction, machine translation. Machine translation needs both halves — understand the source, generate the target — and is filed under generation.

**Language modeling** — predicting the next word from the words before it. It sits among the basic algorithms in the NLP stack, and scaled up enormously it is exactly what today's large language models are.

**Language understanding** (LU / NLU) — text in, label or structure or meaning out: text classification, sentiment analysis, intent recognition, entity extraction. Boundary vs generation: understanding never writes new sentences.

**Large search space** (in speech) — an image classifier picks one of a handful of classes, but speech recognition picks a *sequence* of words from a vocabulary of tens of thousands, so a five-word utterance has roughly 50,000⁵ ≈ 3 × 10²³ possible word sequences. Fixes: restrict the vocabulary to the domain, use a language model to rank plausible sequences, and confirm before acting.

**Low resolution** — a CV challenge: too few pixels land on the object to tell what it is. The arithmetic: shrinking a 1024×1024 image to 256×256 divides each side by four, so a 10×10-pixel vehicle becomes about 2×2 pixels. The step most people miss is *why* the shrinking happens — most models accept only a small fixed input size (224×224, 256×256) because large images are slow and memory-hungry, so the camera's photo is shrunk first, each 4×4 block of 16 pixels averaged into one. The car goes from 100 pixels to 4, and nobody, human or model, can tell car from rickshaw from shadow. Analogy: a whole-class photo sent over WhatsApp, then zoomed in on one face. Fixes: a higher-resolution camera, zoom or closer placement, tiling, or super-resolution as preprocessing. As a requirement it is written as "the smallest object to be detected shall be at least N×N pixels in the image the model receives."

---

## M

**mAP** (mean Average Precision) — the object-detection metric. For each class, detections are ranked by confidence and matched to ground truth by IoU, precision is averaged across recall levels to give that class's average precision, and the mean is taken over classes. mAP@0.5 fixes the IoU threshold at 0.5; the COCO-style mAP@[.5:.95] averages over ten thresholds and is therefore stricter.

**MIoU** (mean IoU) — the segmentation metric: IoU between predicted and true masks, computed per class and averaged over classes.

**Model orchestration** — wiring several models and ordinary code into one pipeline: deciding the order they run in, what each passes to the next, and what happens when one is unsure or fails. One model is rarely an application. The fever-screening case is orchestration end to end: pedestrian detection → face detection → image alignment → thermal measurement (not AI at all) → alarm. Boundary vs training: training builds each model, orchestration connects the finished models into a working service.

**Model validity** — a speech-interaction challenge covering the limits of the model itself: inadequate or dirty training data (too few recordings exist for many regional languages — a data gap, not a property of the language — and wrong transcripts), high output dimensions with a large search space, and the difficulty of integrating outside knowledge such as context, intent and tone.

**MoSCoW priority** — Must, Should, Could, Won't (this time). Used to prioritise functional requirements. "Won't" is the useful one: it records what was deliberately left out — detecting whispering when the cameras have no microphones, or diagnosing illness in a fever-screening system — so that nobody later assumes it was promised.

**Motion blur** — a CV challenge: a fast-moving object, or a shaking camera, smears across many pixels during a single exposure. A speeding car, a cricket ball. Fix: a faster shutter or higher frame-rate camera, plus blurred samples in training.

**Multi-AZ deployment** — the same workload spread across two or more AZs in one region, so that one AZ failing leaves the service up. Typical shape: web ECSs in AZ1 and AZ2 behind an Elastic Load Balancer, with a database whose primary in AZ1 replicates synchronously to a standby in AZ2 that promotes automatically. Costs: duplicated resources, cross-AZ traffic, and a millisecond or two of replication latency. Fit: production, and anything carrying an uptime commitment.

**Multi-class / multi-label classification** — see **Binary, multi-class and multi-label classification**.

**Multi-region deployment** — resources duplicated across separate regions. Boundary vs multi-AZ: multi-AZ survives a *data-centre* failure and is cheap enough to be the production default, while multi-region survives a *whole-region* disaster and also serves distant users or satisfies data-residency law — but it costs far more and forces you to handle high-latency, asynchronous replication. Exam note: high availability → multi-AZ; disaster recovery or compliance → multi-region.

---

## N

**Named entity recognition** (NER) — finding and typing the names in a piece of text. "Ayesha booked Daewoo from Bahawalpur to Lahore on 20 October" yields Ayesha = PERSON, Daewoo = ORG, Bahawalpur and Lahore = LOCATION, 20 October = DATE. It is the core of OCR post-processing, where names and dates have to be pulled out of a scanned form.

**New word discovery** — finding terms that are missing from the dictionary: new slang, brand names, Roman Urdu spellings.

**NLP processing stack** — four layers, read from the bottom up: ① platform — cloud resources and ML/DL methods; ② basic algorithms — the building blocks (word segmentation, POS tagging, syntax analysis, NER, keyword extraction, text similarity, new word discovery, word and sentence vectors, language modeling); ③ capabilities — language understanding and language generation; ④ applications — what the user actually sees. Each application combines boxes from several layers.

---

## O

**Object detection** — mid-level vision: finds every instance of the target classes and returns, for each one, a class, a bounding box and a confidence score. It gives what, where and how many. Boundary vs segmentation: a box is a rectangle that includes some background, while segmentation gives the exact outline. Examples: counting vehicles per lane at a toll plaza, no-helmet detection on a building site. Metric: mAP. Developer side: the YOLO family, Faster R-CNN, SSD, and the ExeML object detection project.

**Object recognition** — an umbrella phrase rather than a separate algorithm: identifying objects in an image, whether by classification or by detection. Note the narrower sense inside product names — in *face recognition*, "recognition" means identifying *which specific instance*: detect the face first, then match it against known identities.

---

## P

**Panoptic segmentation** — semantic and instance segmentation together: uncountable "stuff" (sky, road, grass) labelled semantically, countable "things" (each car, each person) separated individually.

**Part-of-speech (POS) tagging** — labelling each word with its grammatical role — noun, verb, adjective. Often the input to syntax analysis.

**Posture variation** — a CV challenge: the same person takes very different shapes — standing, bending, squatting, lying down. Fall detection has to recognise a lying person, which looks nothing like the standing people that dominate most training sets. Fix: every posture represented in the training data, or pose estimation as an intermediate step.

**PR curve** (precision–recall curve) — precision plotted against recall as the decision threshold moves. Preferred over the ROC curve when the positive class is rare, because it does not flatter a model that is mostly predicting the majority class.

**Precision** — of the items the model labelled X, the share that really are X. It is the metric that matters when false alarms are expensive: every false flag in an exam hall costs an invigilator's attention and a candidate's concentration.

**Product prototype** — what conventional software development designs: a mock-up of the product itself — screens, buttons, workflow, as wireframes or a clickable mock-up. It answers "is this what users want?". Because conventional software behaves predictably, mocking the interface is enough. Boundary vs service solution prototype.

**Proof of concept** (PoC) — the everyday industry name for a service solution prototype: the smallest end-to-end trial that proves, or disproves, that an AI approach works on the client's real data before full development begins. It produces the evidence for the feasibility step of requirement analysis.

---

## R

**RDS** (Relational Database Service) — Huawei Cloud's managed relational database (MySQL, PostgreSQL, SQL Server), holding structured data such as FAQ answers and stored results. Boundary: OBS stores files and objects, RDS stores rows and tables you query, DLI analyses data at scale.

**Real-world data** — recordings of actual events as they happen. Boundary vs staged data (real footage of an acted scene) and synthetic data (generated by a computer).

**Recall** — of the items that really are X, the share the model found. It is the metric that matters when misses are expensive: a fever screen that misses sick people fails at its only job, however few false alarms it raises.

**Region** — a geographic location where Huawei Cloud has infrastructure (CN North-Beijing4, AP-Southeast-Singapore, AF-South-Johannesburg). You choose one for latency to your users, for price, and for data-residency law. Resources in different regions are isolated: an ECS in Singapore cannot attach an EVS disk from Beijing, and traffic between regions crosses the public backbone, taking tens to hundreds of milliseconds.

**ROC curve** — true-positive rate plotted against false-positive rate as the decision threshold moves, summarising how a model trades misses against false alarms across every possible threshold rather than at one.

---

## S

**Scale variation** — a CV challenge: the same object fills a very different share of the frame depending on distance — one vehicle might occupy over 80% of an image or under 10%. Fix: training photos at many distances, and multi-scale feature extraction such as feature pyramid networks.

**Semantic search** — searching by meaning rather than exact keyword, so that "cheap flight" also finds "low-cost airfare". Built on sentence embeddings: both query and documents become vectors, and the closest vectors win.

**Semantic segmentation** — every pixel gets a class label in a single pass. It never asks *which* object a pixel belongs to, so two touching cars come out as one "car" region. It also labels uncountable "stuff" — sky, road, grass, water — which detection boxes cannot represent at all.

**Service solution prototype** — what AI application development designs instead of a product prototype: a small working version of the *whole service* — real sample data → the AI capability (a pre-built API or a quick ExeML model) → how the uncertain output gets used → the people and process around it, who checks and who acts → where it runs. It answers "can the AI do this well enough, and does the process around it cope with its mistakes?". Exam-hall illustration: the product prototype is a mock-up of the invigilator's flag screen, while the service solution prototype is one camera feeding a quick phone-use detector trained on staged clips, raising a flag that an invigilator confirms, run through one mock exam to measure correct flags, delay, and whether invigilators trust it.

**SIFT** (Scale-Invariant Feature Transform) — a classical hand-crafted feature from 2004: it finds distinctive keypoints and describes each by the local gradient directions around it, unchanged by scale and rotation. The pre-deep-learning workhorse for matching and classification. Boundary vs HOG, which describes a whole region rather than individual keypoints.

**Sigmoid** — squashes a single score into a value between 0 and 1, independently of all other classes. It is what makes multi-label output possible, since the scores need not sum to 1. Boundary vs softmax.

**Single-AZ deployment** — every resource sits in one AZ. It is the simplest and cheapest option: no duplicated servers, no cross-AZ traffic charges, lowest latency between tiers. It is also sometimes forced, since an EVS disk attaches only to an ECS in its own AZ. The risk is total — if that AZ goes down, the whole application goes with it. Fit: development and test environments, labs, internal tools, anything where a few hours of downtime is survivable.

**Softmax** — the last step of a multi-class classifier: it turns the raw scores (logits) into probabilities between 0 and 1 that sum to 1, so the classes compete with one another. Boundary vs sigmoid, which scores each class independently for multi-label work.

**Staged data** (acted or simulated recordings) — real recordings of a scene that has been deliberately arranged and acted out. In the exam-hall example, consenting volunteers sit a mock exam in the real halls under the real cameras and act out phone use and note passing on cue. Why use it: real candidates never consented, real cheating is rare so staging produces hundreds of examples on demand, labels are exact because everyone knows when each act happened, and the footage can go to the cloud for training without exposing real exam recordings. Its weakness: actors behave more obviously than real cheaters, so the model may do worse on the real thing — a form of domain shift. Mitigate by briefing volunteers to act naturally, mixing in plenty of ordinary behaviour, and judging the system finally on real pilot results. Boundary vs synthetic data (computer-generated) and real-world data (actual events).

**Super-resolution image reconstruction** — primary vision: rebuilds a higher-resolution image from a low-resolution one by *predicting* plausible fine detail learned from training pairs. Boundary vs ordinary resizing (bicubic interpolation), which only spreads the existing pixels and looks blurry; boundary vs denoising and deblurring, which fix quality at the same size. Important caution: the added detail is generated, not recovered, so an upscaled number plate is not evidence. Examples: 4× upscaling of distant CCTV frames or satellite tiles. Developer side: SRCNN (the first deep SR model, 2014), ESRGAN and Real-ESRGAN; metrics PSNR and SSIM.

**Syntax analysis** (parsing) — working out the structure of a sentence: who did what to whom. The step that separates "the dog bit the man" from "the man bit the dog" when the words alone are identical.

**Synthetic data** — training data generated by a computer: rendered 3-D scenes, or images produced by a GAN or diffusion model. Boundary vs staged data, which is real camera footage of an acted scene.

---

## T

**Text similarity calculation** — scoring how close two pieces of text are in meaning. It powers semantic search, duplicate-complaint detection, and matching a user's question to a stored FAQ.

**Texture** — one of the mid-level image features: a repeating small-scale pattern that characterises a surface — stripes, grass, sand grains, ripples. Grouping pixels of similar texture inside connected edges is what splits an image into regions. Developer side: Gabor filters classically; a CNN's early layers learn texture detectors on their own.

**Thermal image** (infrared image) — an image in which each pixel records the heat a surface gives off, that is its temperature, rather than reflected visible light. It is displayed on a colour scale with hot shown bright. It works in complete darkness but shows little detail — no clear eyes or facial features — and usually has low resolution, often only a few hundred pixels across. Boundary vs **visible-light image**.

**Tiling** — cutting a large image into model-sized pieces (a 1024×1024 photo becomes 16 tiles of 256×256) and running the model on each tile, so that nothing is shrunk and small objects keep all their pixels. Tiles usually overlap slightly so an object sitting on a boundary is not cut in half. Boundary vs resizing: resizing trades detail for speed, tiling keeps the detail and pays for it in model calls — 16 instead of 1.

**Token** (IAM token) — a temporary credential, valid about 24 hours, fetched from the IAM API and carried in the `X-Auth-Token` header. It suits a short session or a service that can refresh it, where AK/SK suits unattended scripts and SDKs. Temporary AK/SK plus a SecurityToken is the short-lived variant for when credentials must be handed to something you do not fully trust.

**Top-1 / top-5 accuracy** — top-1 means the model's single best guess is correct; top-5 means the correct class appears anywhere in its five best guesses. ImageNet results are traditionally quoted as top-5 error.

**Transport channel** — the path audio travels after the microphone: the phone network or a VoIP connection. An ordinary phone call carries only a narrow band of the voice's frequencies, and a weak signal drops pieces of speech outright, so a model trained on clean studio audio performs noticeably worse on calls.

---

## V

**Visible-light image** — an ordinary camera image. Sharp and detailed, good for finding people and faces, but it carries no temperature information. Boundary vs thermal image — the fever-screening system needs both, which is why image alignment is required.

**Visual tracking** (object tracking) — advanced vision: follows objects through consecutive video frames, giving each a persistent ID and a trajectory, and holding that identity through brief occlusion. Boundary vs detection: detection treats every frame independently, so one shopper produces around 900 separate detections in 30 seconds, whereas tracking links them into one identity, one path and one dwell time. Single-object tracking follows one target marked in the first frame; multi-object tracking detects and associates everyone. Examples: unique-visitor counting and dwell time in a store, ball tracking in sports broadcasts. Hard cases are exactly the CV challenges — occlusion, motion blur, scale variation. Developer side: Kalman filter with Hungarian matching (SORT), then DeepSORT and ByteTrack; OpenCV's CSRT and KCF trackers.

---

## W

**WER / SER** — word error rate is the share of words wrongly recognised, counting substitutions, deletions and insertions; sentence error rate is the share of sentences containing any error at all. These are the speech-recognition metrics. A requirement should always state the WER target *together with* the test conditions — noise level, phone line, speaker regions — since the same system can score 5% in a quiet room and 30% on a call.

**Word segmentation** — splitting text into words. It is critical in Chinese, which is written without spaces, which is why it appears so prominently in Huawei material. In English it is mostly straightforward tokenisation. It is genuinely hard in Urdu, where spaces are inconsistently omitted inside words and inserted between parts of one word.

**Word vector / sentence vector** (embeddings) — turning a word or a whole sentence into a list of numbers so that similar meanings land close together in that space. This is the basis of semantic search and text similarity. The classic demonstration: king − man + woman ≈ queen.

---

## Appendix — which metric goes with which task

| Task | Metric |
|---|---|
| Image classification | Accuracy, precision, recall, F1, ROC / PR curves, confusion matrix |
| Object detection | mAP (built on IoU) |
| Image segmentation | MIoU |
| OCR | Character precision rate (CPR), character recall rate (CRR) |
| Speech recognition | WER, SER |
| Machine translation | BLEU |
