# AI-Generated Content Watermarking and Blockchain-Based Royalty Distribution: A Breakthrough for On-Chain AI Verification

## Abstract

Generative AI models can produce text, images, audio and more at scale – bringing innovative opportunities but also raising issues of content authenticity, copyright ownership, and profit distribution [1, 2]. **AI-generated content watermarking** has emerged as a key technique for embedding hidden markers in AI outputs to facilitate source tracing and model identification [3]. This paper presents a comprehensive overview of current watermarking methods for text, image, and audio content, and proposes using these watermarks to build an **on-chain content provenance system** that traces model usage and content dissemination. On this basis, we design a blockchain-based **royalty distribution mechanism**: by leveraging tamper-proof blockchain records and watermark detection, we enable automatic back-tracing of contributions and revenue sharing across multiple rounds of AI-generated content [4, 5, 6]. We detail watermark embedding techniques, robustness and anti-tampering measures, digital signature integration, blockchain registration, and smart contract structures [7, 8, 9]. We also analyze potential attack vectors and the technical feasibility of the system. Finally, we discuss the scheme's application prospects in the digital content industry, its business significance, and investment value – highlighting that lightweight watermark-based verification could be a breakthrough for bringing AI content on-chain (solving the impracticality of traditional heavy zero-knowledge proofs) and for streamlining royalty computations.


## 1. Introduction

Recent breakthroughs in generative AI (for chat, image synthesis, voice cloning, etc.) have heightened concerns about content authenticity and copyright attribution. On one hand, AI-generated content can be misused for deepfakes and misinformation, eroding public trust in digital media [1]. For example, in 2023 an AI-generated image of Pope Francis in a puffy coat fooled millions of viewers before being revealed as fake [11], and criminals have used AI-cloned voices of company executives to commit fraud [12]. These incidents underscore the risks and societal harm when content provenance is unverified. On the other hand, large generative models are trained on vast internet datasets that include many copyrighted works, sparking an outcry from creators demanding compensation and attribution [2]. Artists and authors have filed lawsuits alleging that AI companies used their works without permission. How to encourage technological innovation while protecting creator rights and ensuring AI-generated content's credibility has become an urgent industry and regulatory question [3].

To address these challenges, the industry has proposed embedding **digital watermarks** in AI-generated content. A **digital watermark** is a hidden identifier embedded imperceptibly into digital content to mark the content's source or owner [3]. Watermarking promises that, without affecting user experience, we can tag AI-generated text, images, audio, etc. with an invisible signature that is machine-detectable. In combination with distributed ledger (blockchain) technology, these watermark identifiers can be linked to on-chain content credentials (e.g. generator ID, timestamps, copyright info), creating an end-to-end content provenance framework [7, 13]. As AI-generated content spreads online, anyone can extract the watermark and check the blockchain record to verify who (or which model) produced the content and through what transformations it has passed.

Going further, blockchain smart contracts enable automated royalty-sharing. Whenever AI-generated content is re-used, remixed, or commercially exploited, a smart contract can, based on the on-chain provenance graph and preset profit-sharing rules, automatically allocate revenue to relevant contributors (such as original data creators, model developers, downstream editors, etc.) [14, 15]. This mechanism could resolve the current problem where **data providers receive no benefit** from generative AI – instead establishing a healthy AI content ecosystem where value flows back to original creators.

## 2. Technical Background

### 2.1 Digital Watermark Concept and Characteristics

A **digital watermark** is a technology for embedding hidden identifiers into digital content for copyright marking and content authentication [3]. A typical watermark system involves **watermark embedding** and **watermark detection/extraction**. The encoder merges identifier information (e.g. owner ID, content ID) with the original content to produce a watermarked piece; the decoder extracts or verifies the hidden identifier to confirm its source [3,10].

An ideal digital watermark should satisfy:
- **Imperceptibility**: embedding should not cause perceptible quality degradation [16]
- **Robustness**: the watermark should remain detectable even after common transformations (re-compression, cropping, noise, format conversion) [3]. Watermarks are classified as *robust* (survive moderate edits) or *fragile* (destroyed by tiny changes, used to detect tampering)
- **Security**: watermark schemes use secret keys – only those with the key can embed or detect the watermark. Unauthorized attackers should neither easily detect nor remove it without degrading content [17]. A secure watermark should be **unforgeable**: computationally infeasible to create valid watermarked content without the secret key [18]

### 2.2 Watermark vs. Digital Fingerprint

Digital watermarks embed the **same identifier** in all copies of content to label source or copyright. A **digital fingerprint** embeds a **unique hidden ID in each individual copy** to trace which specific copy was leaked [19,20]. Our system focuses on watermarks for source authentication (marking AI outputs with model ID), though the framework could extend to fingerprinting for tracing distribution channels.

### 2.3 Multi-Modal Watermarking Techniques

Digital watermarking is well-established for images and video. Classic methods work by adding imperceptible noise in transform domains (DCT, wavelet) or altering least significant pixel bits [21]. Advanced strategies integrate watermarks into the generative model itself: Wen et al. (2023) propose "Tree-Ring Watermark" for diffusion models that injects a circular interference pattern into initial noise, causing every generated image to carry a hidden spectral pattern [22]. Other approaches root watermarks directly in the latent diffusion models [23] or propose recipes for watermarking diffusion models [24].

For audio watermarks, techniques leverage human hearing masking effects, embedding signals in less audible frequency ranges or using phase modulation [25]. For text watermarking, sophisticated approaches bias language model word choices to encode statistical patterns – Kirchenbauer et al. (2023) pioneered a method using a secret key to designate "green-list" words, nudging the model to prefer them such that the distribution reveals a hidden signature [26, 27]. Google DeepMind's SynthID-Text similarly acts as a logits processor [28, 29]. Text watermarks face fragility challenges: paraphrasing can disrupt statistical markers [27].

### 2.4 New Developments in Watermarking

Recent advances apply deep learning to watermarking, enabling end-to-end optimization of encoder-decoder pairs for embedding and detection after distortions [3]. Google's SynthID [16, 28] and Meta's VideoSeal [30] use neural networks to encode signatures directly into content imperceptibly, withstanding blurring, cropping, re-encoding [16]. These are trained adversarially: watermarked content undergoes simulated attacks, and the decoder learns to still extract the watermark. Gunn et al. (2024) propose using pseudo-random error-correcting codes in diffusion model latent space to embed encrypted signatures across semantic levels, making watermarks undetectable without the key [31, 32]. There are even methods emerging for universal watermarking across different image resolutions [33].

Another research line focuses on anti-forgery and public verification. Liu et al. (2024) propose "unforgeable and publicly verifiable watermarks for LLMs" by separating generation and detection with different keys, leveraging cryptographic principles to provide security guarantees [17, 18]. Other work focuses on semantic-invariant watermarks robust to paraphrasing [34, 35]. These approaches aim for reliable and trustworthy watermarking with provable security properties [32, 36]. However, some research also highlights that invisible image watermarks may be provably removable with generative AI [37].

## 3. Watermark Techniques and Properties

Having covered the general concepts, we now examine current mainstream watermarking methods for different content modalities and compare their properties, laying the groundwork for our on-chain system design.

### 3.1 Text Watermark

Watermarking AI-generated text embeds barely perceptible patterns in LLM outputs. Two primary approaches:

**(a) Invisible character embedding**: inserting zero-width Unicode characters to encode information. Simple but easily removed, so robustness is low.

**(b) Probability distribution encoding**:  tweaking word selection to imprint a statistical signature. OpenAI's prototype uses a secret key to classify vocabulary into "green" and "red" lists, biasing toward green-list words [26, 27, 38]. This is invisible to readers and maintains fluency. However, text watermarks are **fragile**: paraphrasing or regeneration can destroy the signal [27]. Research to improve robustness includes larger n-gram patterns or syntactic watermarking, but there's a trade-off between detection reliability and vulnerability to removal. Google's DeepMind has integrated SynthID-Text into Gemini [28, 39], and OpenAI has hinted at watermarking [40, 41]. More robust schemes with cryptographic assurances [18, 34] and open-source toolkits [42] are anticipated.

### 3.2 Image Watermark

Digital watermarking is most established for images. Traditional methods embed watermarks in frequency domains (DCT coefficients, LSB flips) [21]. Learning-based approaches now dominate: generative models are augmented so **every generated image contains a hidden watermark by design**. Google's **SynthID** adds an invisible, hard-to-remove pattern that doesn't affect image quality but remains detectable after cropping, re-scaling, or filters [16]. SynthID uses adversarial training to ensure robustness [28]. The system relies on secure keys rather than algorithm secrecy – even knowing the algorithm, attackers without the key cannot forge or erase the watermark without damaging the image [17]. Other novel methods include dynamic watermarks [43] and "tree-ring" watermarks [22].

**Video watermarking** extends image techniques. Meta's **VideoSeal** embeds signatures across video frames using deep learning, optimized for video-specific distortions (compression, frame rate changes) [30]. It achieves robust detection even with cropping or dropped frames by leveraging temporal redundancy.

### 3.3 Audio Watermark

Audio watermarking leverages human hearing **masking effects**: modifications in ranges where louder audio dominates go unnoticed. Common methods include adding low-level broadband noise shaped to the audio spectrum, introducing faint echoes, or tweaking phase [25]. A robust audio watermark should withstand re-sampling, **lossy compression** (MP3/AAC), ambient noise, and speed/pitch adjustments. Advanced schemes use **spread spectrum** signals – pseudorandom sequences spanning wide frequency ranges at low power, requiring destruction of large audio portions to remove [21]. Error-correcting codes help recover messages even with partial loss. The human ear's sensitivity requires extreme subtlety – poorly implemented watermarks creating audible artifacts are unacceptable. Recent developments include watermarking integrated with AI **vocoders** or speech synthesizers, constraining the model's output layer to produce watermarked audio from the start, potentially yielding more hidden yet resilient marks.

### 3.4 Multi-Modal & Metadata Watermark

Beyond altering content signals, methods based on metadata exist. The **C2PA standard (Coalition for Content Provenance and Authenticity)** allows producers to attach cryptographic metadata "credentials" to files, recording creator, editing history, or AI-generation status. This has **zero quality impact** but metadata can be easily stripped. A practical strategy **combines invisible watermarks with metadata signatures**: the watermark serves as a hidden backup, while metadata provides a convenient provenance trail. Numbers Protocol creates a hash "fingerprint" registered on-chain plus C2PA metadata; if metadata is stripped, the hash or watermark can still match against the blockchain [13].

A watermark serves as a **content-level ID tag**, while blockchain serves as an **immutable registry**. This multi-pronged strategy provides defense in depth.


## 4. On-Chain System Architecture

Figure 1 below illustrates the overall architecture of our proposed on-chain provenance system and the information flow between its components:

```
┌─────────────────────────────────────────────────────────────────────┐
│                    AI-GENERATED CONTENT PROVENANCE SYSTEM           │
└─────────────────────────────────────────────────────────────────────┘

STEP 1: CONTENT GENERATION & WATERMARKING
┌──────────────────┐
│  AI Model /      │
│  Creator         │───────┐
└──────────────────┘       │
                           ▼
                    ┌─────────────────────┐
                    │  Generate Content   │
                    │  with Embedded      │
                    │  Watermark ID       │
                    └──────────┬──────────┘
                               │
                               ▼
STEP 2: BLOCKCHAIN REGISTRATION
                    ┌─────────────────────┐
                    │  Register Content   │
                    │  ID & Metadata      │
                    │  on Blockchain      │
                    │  (Provenance Record)│
                    └──────────┬──────────┘
                               │
                               ▼
STEP 3: CONTENT DISTRIBUTION
                    ┌─────────────────────┐
                    │  Content Shared/    │
                    │  Published to       │
                    │  Users/Platforms    │
                    └──────────┬──────────┘
                               │
                               ▼
STEP 4: PROVENANCE VERIFICATION
                    ┌─────────────────────┐
                    │  Extract Watermark  │
                    │  ID from Content    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Query Blockchain   │
                    │  using Content ID   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Return Source &    │
                    │  History Info       │
                    │  (Model, Creator,   │
                    │  Timestamp, etc.)   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Display Provenance │
                    │  & Attributions     │
                    └─────────────────────┘

STEP 5: DERIVATIVE CREATION (Optional)
┌──────────────────┐
│  Derivative      │
│  Content Input   │───────┐
└──────────────────┘       │
                           ▼
                    ┌─────────────────────┐
                    │  New AI Model/      │
                    │  Editor Generates   │
                    │  Derivative         │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Embed New          │
                    │  Watermark ID       │
                    │  (links to parent)  │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Register New ID    │
                    │  with parent        │
                    │  reference on       │
                    │  Blockchain         │
                    └─────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│              ROYALTY DISTRIBUTION (Smart Contract)                  │
│                                                                     │
│  When content generates revenue (sale, licensing, ad revenue):      │
│  1. Smart contract reads provenance graph from blockchain           │
│  2. Calculates royalty splits based on recorded percentages         │
│  3. Automatically distributes payments to all contributors          │
│     (original creator → intermediate editors → current owner)       │
└─────────────────────────────────────────────────────────────────────┘
```

**Figure 1: Architecture of the watermark-based on-chain content provenance system.** When a generative AI model produces content, it embeds a watermark containing a unique content identifier (ID). That ID, along with relevant metadata, is immediately recorded on the blockchain (provenance registration). If the content is later edited or used to create new content, the new AI (or user) preserves the original watermark or embeds a new one, and links the new content's record on-chain to its source. When the content is disseminated to end-users or platforms, anyone can extract the watermark ID and query the blockchain to retrieve the content's origin, its full creation history, and associated rights information. This enables verification of source and tracking of derivative lineage through immutable records. Smart contracts can then use the on-chain records to automate royalty settlements based on the provenance graph.

### 4.1 Content Registration & Watermark Embedding

Whenever an AI model (or human creator) generates a new piece of content, the system first assigns it a globally unique identifier – this could be a UUID or, more securely, a cryptographic hash of the content file. Next, a watermark corresponding to this ID is embedded into the content using one of the techniques discussed (text watermark for text, image watermark for images, etc.). For example, if the model generates an image, the system might embed a certain pseudo-random pattern in its pixel domain representing the ID; if the output is text, it might apply a lexical watermarking with a key tied to the content ID; if audio, it adds an inaudible signal encoding the ID. Crucially, this watermark is invisible to users but will later allow anyone to recover the content ID from the content itself. After embedding, the system creates a **blockchain transaction** to record the content's provenance metadata. This on-chain **content record** would typically include: the content's unique ID (or a hash), the creator or generating model's identity, a timestamp of creation, possibly the prompt or source data ID used (if such information is to be shared), and any initial usage rights or license terms (e.g. "© Creator, non-commercial use only" or an NFT ownership token reference). The transaction is sent to the blockchain (which could be a public blockchain or consortium ledger) and once confirmed, it provides an immutable, timestamped proof of the content's origin. It's important to note that we do **not** need to store the actual content file on-chain – we only store a hash or ID and metadata. The content itself can be stored off-chain in a distributed storage network (like IPFS/Filecoin) or a cloud server, with the content hash ensuring integrity. By comparing a file's hash to the on-chain hash, one can verify the file hasn't been altered. Thus, at the moment of creation, every AI-generated content gets a kind of "birth certificate" on the blockchain and a hidden "ID tag" within the content via the watermark.

### 4.2 Content Dissemination & Provenance Query

When watermarked AI content circulates on the internet or is shared to a platform, any recipient can verify its provenance by extracting the watermark. For instance, suppose a user encounters an image or audio clip and wonders if it's AI-generated and from which model. They can use a detection tool (this could be built into social media platforms, browser extensions, or standalone apps) to scan the content for a watermark. If a watermark ID is found, the tool then queries the blockchain (either by searching for that content ID or calling a smart contract function) to retrieve the corresponding provenance record. The blockchain lookup will return the stored metadata: e.g. "Content ID X was generated by Model Y (version Z) at time T; Creator=Alice's AI Studio; Original hash H; License=CC-BY-NC," etc. It may also indicate whether the content was derived from prior content (more on that shortly). This allows the viewer to confirm **who or what model produced the content and when**. If the chain record shows it's AI-generated by a particular model, the user or platform can then, for example, display an "AI-generated" label or decide to treat it accordingly. This automated provenance check is valuable for **content authenticity** – e.g., a news outlet receiving a photo can verify if it comes from a known journalist's camera (with a registered record and no AI watermark) or if it was AI-generated by some model, helping to prevent deceptive deepfakes in journalism. For regulators or copyright enforcers, the system provides a transparent way to trace content: if infringing AI content appears, one can find out which model made it (and potentially which data it used, if those details are recorded or can be further queried). Importantly, because the watermark is embedded in the content itself, the provenance stays attached to the content wherever it goes. Anyone, even years later, can perform the check as long as the blockchain records persist.

### 4.3 Authenticity Verification & Tamper Detection

Because content can be tampered with (including malicious attempts to remove or alter a watermark), the architecture includes mechanisms for authenticity verification using the blockchain records. One straightforward measure is storing the content's cryptographic hash on-chain as part of the provenance record. When a watermark is extracted, one can compute the current content file's hash and compare it to the on-chain hash recorded at creation. If they differ, it means the content has been modified since it was registered (or it's not the exact original file). This acts as an integrity check – for example, if someone tries to subtly alter a watermarked image (without completely destroying the watermark) and pass it off as the original, the hash mismatch will reveal the change. Another sophisticated approach proposed by industry (e.g. by Digimarc) is to embed not just an ID but also a **content fingerprint within** the watermark itself. For instance, each video frame's watermark payload might include a hash of that frame's pixels. If an attacker alters the frame, the extracted hash won't match the actual frame content, signaling tampering. The blockchain record could store expected hashes or even AI-generated "reference features" (like key image descriptors) of the original content. Upon verification, one compares the current content's features to the recorded ones. If there's a discrepancy, it indicates manipulation. This dual approach – watermark + on-chain hash – creates a **belt and suspenders** security model. Even if an attacker manages to **forge a watermark** or copy a valid watermark from one content to another, they would fail to produce a matching hash to the blockchain record, since the content itself differs. The immutable timestamped nature of blockchain also helps mitigate certain attacks: for example, an attacker cannot easily create a fake chain record retroactively with an earlier timestamp to claim origin, nor can they alter an existing record to cover their tracks. Every update (like a derivative registration) is append-only and time-stamped, providing a chronological audit trail. In case of dispute (say someone claims a piece of content is human-made when chain says AI-generated), the combination of watermark evidence and blockchain proof-of-record can serve as a **strong digital witness**. Users can always rely on the chain data because it's decentralized and tamper-proof, unlike a centralized database that could be hacked or altered. In summary, the on-chain provenance architecture leverages watermarks as invisible "content passports" and blockchain as a secure "passport registry": the watermark travels with the content, and the blockchain provides a trustworthy database to validate those passports. Together, these enable open-world verification of digital content origin and integrity in a way that was previously not possible.

## 5. Royalty Distribution Mechanism

Building on the content provenance foundation, we introduce a **royalty distribution mechanism** that ensures participants in the AI content creation chain are compensated fairly whenever the content generates value. This mechanism has two main parts: on-chain licensing records and automatic revenue splitting via smart contracts.

### 5.1 On-chain License Registration

When a piece of content is registered on-chain (as described above), the creator or rightsholder can attach a digital copyright notice or usage license terms to its blockchain record. This could be implemented as part of the content's metadata or via a separate smart contract that maps content IDs to royalty rules. For example, suppose Alice creates image A (ID=A) using her AI model. She might specify on-chain: "If this image (or its derivatives) is used commercially or sold, 5% royalty goes to Alice." Another example: a model developer might declare that any output from Model X carries a baseline royalty of 2% to the model's owner. To facilitate interoperability, these terms should use a standard schema – for instance, a **programmable license template**. The emerging **Story Protocol** has proposed a concept of programmable IP licenses (PIL) that allow creators to encode permissions and royalty percentages for derivatives. Using such a template, Alice could signal that derivative works are allowed as long as a certain revenue share is paid. All such license and royalty data is anchored on-chain alongside the content's provenance. Importantly, the blockchain record will also record the **current owner or royalty recipient address** for the content. This could simply be the wallet address of the original creator by default, but it could be transferable (for example, if Alice sells the rights to Bob, the on-chain owner could be updated to Bob's address). By having each content's rights and royalty info on-chain, we enable smart contracts to later automatically calculate payouts by reading those records.

### 5.2 Triggering Events & Royalty Computation

When a content is monetized – for instance, sold as an NFT, licensed for use in a game, or generating ad revenue on a platform – an event is sent to the blockchain to share the revenue among entitled parties. Take a simple case: content C (ID=C) is a derivative of B (parent) and A (grandparent). Now someone purchases content C's NFT for 100 tokens, or perhaps content C generates 100 tokens of advertising revenue on a platform. We have on-chain rules (from earlier registrations) that say, for example, "A gets 5% of derivative sales" and "B gets 10%". A specialized **royalty smart contract** is deployed which knows how to traverse the content lineage graph and gather applicable royalties. When the sale occurs, the platform or marketplace contract would call the royalty contract, passing in content C's ID and the payment amount (100). The royalty contract looks up C's on-chain record to see if it has any parent content. It finds B as parent and A as grandparent via the links. It then looks up B's royalty rule (say B's creator set 10%) and A's royalty rule (5%). It sums these into a total royalty obligation (in this case 15% of the sale). The contract would then split the 100 tokens: 5 tokens routed to A's registered payout address, 10 tokens to B's address, and the remaining 85 tokens to the seller (C's owner). This forms a **"royalty stack"** – each ancestor in the content's provenance chain contributes a slice according to their defined percentage. If any content in the chain had opted out of royalties (e.g. declared a public domain or 0% royalty license), the contract would note that as 0%. If multiple rules apply (say the model demands 2% and the artist demands 5% on the same piece), those could be aggregated or given priority according to the license agreements (potentially handled via more complex logic or tiered contracts). The entire calculation and transfer of funds happens automatically and near-instantly on-chain once triggered. By programming this into a contract, we remove the need for manual royalty accounting or trust in intermediaries – creators get their share directly according to code.


### 5.3 Royalty Vaults and Withdrawals

To manage continuous or streaming revenue (for instance, a piece of content earning ad revenue over time), we can use on-chain **royalty vaults** for each content. Essentially, each content or creator can have an escrow pool in the smart contract where their accrued royalties are collected. Every time a royalty payment is triggered for content A, instead of transferring to A's owner immediately (which could be many small micro-transactions), the funds could accumulate in A's "vault". The owner of A (the address on record) can then withdraw from their vault whenever they choose. This approach is useful if there are frequent tiny transactions – it reduces gas fees by not forcing a token transfer every single time, at the cost of the owner having to call a withdraw function occasionally. The vault concept also allows us to introduce **royalty tokens**. Each content could be associated with a fixed supply of tokens that represent fractional entitlement to its royalty stream. For example, when content A is created, the contract could mint 100 "A-royalty tokens", each representing 1% of the royalty earnings from A. Initially, these tokens belong to A's creator. The creator could then sell some of these tokens to investors or collaborators. Whoever holds these tokens can claim the corresponding percentage of the funds in A's royalty vault. This essentially **tokenizes the future royalty flow** of content A, allowing creators to **monetize upfront or share ownership**. For instance, an AI model developer might sell 30% of the royalty tokens for the model's outputs to raise capital; buyers of those tokens would then automatically receive 30% of any royalties generated by that model's outputs (the contract would split the model's vault accordingly). Story Protocol's design alludes to such **royalty-bearing tokens or NFTs** to enable investment in IP rights. Our system can support this by tying the vault payout to token ownership. All the accounting is transparent on-chain: one can see how much revenue has been accumulated and which addresses or token holders have what shares.

### 5.4 Multi-Party Split Example

Consider a complex scenario to illustrate the logic. Original image A (by Alice) sets 5% royalty. Image B is created from A by Bob; Bob sets an 8% royalty on B. Then image C is generated by Charlie using B (and indirectly A), and Charlie sets 10% on C (this might apply to further derivatives if any). Now, if image C is sold for 1000 tokens, the smart contract sees: A deserves 5%, B deserves 8% – totaling 13%. It will therefore allocate 50 tokens to A's vault (5%), 80 tokens to B's vault (8%), and the remaining 870 tokens to C's owner (Charlie). Alice and Bob can withdraw their earnings from their respective vaults at leisure. If image C later generates another 500 tokens (say licensing fee), the same percentages apply: 25 to Alice, 40 to Bob, 435 to Charlie. This way, even upstream contributors who are **two steps removed** still benefit from downstream success. The process is fully **automated and transparent** – all transactions are recorded on-chain, so each party can verify they got the correct share. This **multi-round revenue backtracking** ensures that "whoever contributed gets to share the reward," which could solve the current pain point where data providers or intermediate creators get nothing when their work is remixed by AI. Instead, they would receive ongoing royalties, creating a positive incentive loop. From an implementation perspective, one can imagine each piece of content as an NFT that has associated royalty rules and beneficiary addresses. Many NFT marketplaces already support automatic royalty payouts for secondary sales (commonly used to give artists a cut of resales); our system generalizes this across **arbitrarily many "generations"** of content via on-chain provenance. The key difference is the chaining of royalties: NFT standards like ERC-721 typically handle a royalty to the immediate creator, whereas here the contract recursively distributes to **all relevant predecessors** based on their terms. This is what Story Protocol and similar projects are aiming to achieve, and we provide a concrete scheme to do so.

In summary, by marrying watermark-based provenance with programmable smart contracts, we build an **automated, fair royalty distribution network** that embodies the principle "whoever contributes to an AI-generated work is rewarded." This benefits creators (who are no longer cut out of the value chain) and also investors (who can fund creative endeavors in return for a slice of future earnings via royalty tokens). It introduces new business models: for example, a photographer could allow her images to be used in model training in exchange for on-chain royalties from any model outputs that include her work – turning AI from a threat into a source of passive income. We will next consider the technical and security challenges in implementing this system and how to mitigate them.

## 6. Business and Market Impact

### 6.1 Zero-Knowledge Proofs and the Computational Bottleneck

One of the most significant technological barriers to bringing AI on-chain has been the computational challenge of verification. Traditional approaches to verifying AI model execution using **zero-knowledge proofs (ZK proofs)** face severe practical limitations. ZK proofs allow one party to prove to another that a computation was performed correctly without revealing the computation's inputs or intermediate steps. In theory, this would be ideal for AI: a model provider could prove "I ran this specific AI model to generate this content" without exposing the model weights or training data.

However, the reality is that **verifying AI model inference through conventional ZK circuits requires enormous computational resources**. Modern large language models involve billions of parameters and trillions of floating-point operations for a single inference. Translating these operations into ZK-compatible arithmetic circuits and generating proofs for them would require computational power orders of magnitude greater than the original inference itself. Current estimates suggest that ZK-proving a single forward pass through a model like GPT-4 could take hours or days on specialized hardware and cost thousands of dollars in compute resources. This **computational bottleneck makes direct ZK verification of AI models economically infeasible and impractical** for real-world applications.

### 6.2 Watermarks as a Breakthrough for On-Chain AI

The emergence of robust digital watermarking represents a **paradigm shift** that makes on-chain AI verification practical and economically viable. Instead of verifying the entire AI model computation, we only need to verify the presence of a lightweight watermark embedded in the output. This transforms the verification problem from:

**"Prove that model M with weights W, given input I, produced output O"** (computationally intractable)

to:

**"Prove that output O contains valid watermark signature S"** (computationally trivial)

Watermark verification is several orders of magnitude lighter than full model verification. A watermark detection algorithm might require only milliseconds of computation on standard hardware, compared to the hours or days required for ZK proof generation and verification of the full model. The cost difference is similarly dramatic – from thousands of dollars per verification down to fractions of a cent.

This lightweight verification enables true **on-chain AI provenance at scale**. Instead of expensive, impractical ZK circuits for model inference, we can use simple, efficient smart contracts that verify watermark signatures. These contracts can quickly confirm that content was generated by a specific registered model, without needing to reconstruct or verify the model's actual computations. This makes it feasible to verify millions of AI-generated outputs daily on a public blockchain, something that would be impossible with traditional ZK approaches.

The watermark-based approach represents an **important milestone for bringing AI truly on-chain**. For the first time, we can have cryptographically verifiable, decentralized tracking of AI-generated content without requiring prohibitive computational resources. This enables the full vision of on-chain AI ecosystems: transparent model attribution, verifiable content provenance, and automated royalty distribution – all at scale and at sustainable cost.

### 6.3 Streamlined Royalty Computation and Payment

Beyond verification, watermark-based provenance dramatically simplifies **royalty calculation and distribution**. In traditional digital content licensing, tracking usage and computing royalties requires extensive manual accounting, auditing, and trust in centralized intermediaries. Disputes over attribution and payment splits are common and expensive to resolve.

With watermark-based on-chain provenance, royalty computation becomes **automated and transparent**. The blockchain maintains an immutable record of content lineage – which model generated it, which data contributed to training, which creators provided source material. Smart contracts can traverse this provenance graph and automatically calculate each party's share based on pre-agreed percentages. When content generates revenue (through sales, licensing, advertising, etc.), the smart contract immediately splits the payment according to the on-chain rules and distributes funds to all contributors' wallets.

This automation has profound implications:

**Cost efficiency**: No need for expensive auditing firms or payment processors. Smart contracts execute automatically with minimal gas fees (often less than a dollar per transaction).

**Transparency**: All parties can verify the royalty calculations and payment history on-chain. There's no "black box" accounting where creators must trust platforms to pay them fairly.

**Real-time payments**: Contributors receive their share instantly when revenue is generated, rather than waiting for quarterly reports and delayed payments.

**Multi-generational tracking**: The system naturally handles complex scenarios where content is derived from other content multiple times. Even if content is remixed through five generations, the smart contract can trace back through the entire lineage and ensure all contributors receive their fair share.

**Global scale**: The blockchain-based system works identically whether the content is sold in New York, Tokyo, or Lagos. There's no need to navigate different legal jurisdictions, currencies, or payment systems.

For business models, this creates **unprecedented opportunities**:

- **Data providers** can monetize their contributions to AI training datasets by receiving automated royalties every time models trained on their data generate revenue
- **Model developers** can guarantee fair compensation through on-chain royalty rules, making it economically rational to share or license their models
- **Content creators** can allow AI remixing of their work while ensuring they benefit from derivative creations
- **Investors** can fund AI development and content creation by purchasing royalty-bearing tokens, creating liquid markets for AI intellectual property

The combination of lightweight watermark verification and automated on-chain royalty computation fundamentally changes the economics of AI-generated content. It removes the technical and economic barriers that have prevented truly decentralized AI ecosystems, paving the way for new business models where value flows fairly to all contributors.

## 7. References

[1] Cloudflare Blog. "AI-generated content watermarking and authentication." blog.cloudflare.com

[2] Reuters. "Artists sue AI companies over copyright infringement." reuters.com

[3] Zhao, X., et al. "SoK: Watermarking for AI-Generated Content." arXiv:2411.18479 (2024).

[4] Balan, K., et al. "Content ARCs: Decentralized Content Rights in the Age of Generative AI." arXiv:2503.14519 (2025).

[5] Zhang, W., Zhang, Q., Zhu, Y. "Blockchain-Based Protected Digital Copyright Management with Digital Watermarking." Journal of Systems Science and Information (2020).

[6] Li, Y., Zhang, H., Wang, K. "WFB: Watermarking-based Copyright Protection Framework for Federated Learning Model via Blockchain." Scientific Reports 14, 17237 (2024).

[7] Lüthi, P., et al. "Distributed Ledger for Provenance Tracking of AI Assets." arXiv:2002.11000 (2020).

[8] Ahmed, I., et al. "Blockchain for video watermarking: An enhanced copyright protection approach for video forensics based on perceptual hash function." PLOS ONE 19(10): e0308451 (2024).

[9] Chen, Y., et al. "A novel blockchain-watermarking mechanism utilizing interplanetary file system and fast walsh hadamard transform." Scientific Reports 14, 22015 (2024).

[10] Parola Analytics. "Countering deepfakes using digital watermarks and blockchain technology." Parola Blog (Aug 11, 2021).

[11] CBS News. "AI-generated Pope Francis image fools millions." cbsnews.com

[12] Avast Blog. "Criminals use AI voice cloning for fraud." blog.avast.com

[13] Numbers Protocol. "Blockchain-based content provenance." numbersprotocol.io

[14] Balan, K., Gilbert, A., & Collomosse, J. Content ARCs: Decentralized Content Rights in the Age of Generative AI. ArXiv. arXiv:2503.14519(2025)

[15] Makridis CA, Ammons JD. Governing the large language model commons: using digital assets to endow intellectual property rights. Journal of Institutional Economics. 2025;21:e19. 

[16] The Verge. "Google's SynthID watermark for AI images." theverge.com

[17] Brooks-Mejia, T., Patton, C. "An early look at cryptographic watermarks for AI-generated content." Cloudflare Blog (Mar 19, 2025).

[18] Liu, A., et al. "An Unforgeable Publicly Verifiable Watermark for LLMs." ICLR 2024.

[19] Custos Tech. "Digital fingerprinting for content tracking." custostech.com

[20] Custos Tech. "Forensic watermarks explained." custostech.com

[21] Saini, L. K., & Shrivastava, V. A Survey of Digital Watermarking Techniques and its Applications. ArXiv. arXiv:1407.4735

[22] Wen, Y., et al. "Tree-Ring Watermarks: Invisible Fingerprints for Diffusion Images." NeurIPS 2023.

[23] Fernandez, P., Couairon, G., Jégou, H., Douze, M., Furon, T. "The Stable Signature: Rooting Watermarks in Latent Diffusion Models." ICCV 2023, pp. 22466-22477.

[24] Zhao, Y., Pang, T., Du, C., Yang, X., Cheung, N.-M., Lin, M. "A Recipe for Watermarking Diffusion Models." arXiv:2303.10137 (2023).

[25] Reddit. "Audio watermarking frequency techniques." reddit.com

[26] Kirchenbauer, J., Geiping, J., Wen, Y., Katz, J., Miers, I., Goldstein, T. "A Watermark for Large Language Models." ICML 2023, pp. 17061-17084.

[27] TechCrunch. "OpenAI's attempts to watermark AI text hit limits." TechCrunch (Dec 10, 2022).

[28] Dathathri, S., et al. "Scalable watermarking for identifying large language model outputs." Nature 634(8035), 818-823 (Oct 2024).

[29] Spectrum IEEE. "Google DeepMind SynthID for text." spectrum.ieee.org

[30] Fernandez, P., et al. "VideoSeal: Open and Efficient Video Watermarking." arXiv:2412.09492 (2024).

[31] Gunn, S., Zhao, X., Song, D. "An Undetectable Watermark for Generative Image Models." arXiv:2410.07369 (2024).

[32] Christ, M., Gunn, S. "Pseudorandom Error-Correcting Codes." IACR Cryptology ePrint Archive 2024/235 (2024).

[33] Bui, T., Agarwal, S., Collomosse, J. "TrustMark: Universal Watermarking for Arbitrary Resolution Images." arXiv:2311.18297 (2023).

[34] Liu, A., Pan, L., Hu, X., Meng, S., Wen, L. "A Semantic Invariant Robust Watermark for Large Language Models." ICLR 2024.

[35] Christ, M., Gunn, S., Zamir, O. "Undetectable Watermarks for Language Models." COLT 2024, pp. 1125-1139.

[36] Nandi, S. "Enhancing AI Transparency: Researchers Develop Advanced Watermarking Techniques." AZoAI News (Dec 5, 2024).

[37] Zhao, X., et al. "Invisible Image Watermarks are Provably Removable Using Generative AI." arXiv:2306.01953 (2023).

[38] TechCrunch. "Statistical watermark detection methods." techcrunch.com

[39] Spectrum IEEE. "DeepMind watermark in Gemini." spectrum.ieee.org

[40] Life Architect. "OpenAI watermarking hints." lifearchitect.ai

[41] Life Architect. "Transparency in AI outputs." lifearchitect.ai

[42] Pan, L., et al. "MarkLLM: An Open-Source Toolkit for LLM Watermarking." EMNLP 2024 System Demonstrations, pp. 61-71.

[43] Chen, Y., et al. "Dynamic Watermarks in Images Generated by Diffusion Models." arXiv:2502.08927 (2025).

