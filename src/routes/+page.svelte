<script lang="ts">
    import {slide} from 'svelte/transition';

    const projects = [
        {
            title: 'Airbnb SP — Pipeline & Dashboard',
            description: 'End-to-end data pipeline and analytics dashboard for Airbnb listings in São Paulo, built on a Medallion Architecture over AWS.',
            tags: ['dbt', 'AWS Athena', 'evidence.dev'],
            rotate: 'rotate-1',
            details: 'Bronze layer with raw listings on S3, Silver layer modeled in dbt on top of Athena (price parsing, type casting, data quality tests), and a Gold layer (dim_listings) queried by Evidence.dev. Evidence caches the Gold layer locally in DuckDB (npm run sources) for millisecond-level dashboard performance, and the dashboard is deployed as a static site (SSG) on Vercel.',
            links: [
                {label: 'repository', href: 'https://github.com/luccappaz/AirbnbWithDBT'},
                {label: 'dashboard', href: 'https://airbnb-with-dbt-xi.vercel.app'}
            ]
        },
        {
            title: 'MovieLens — Data Lakehouse & RAG Recommendation System',
            description: 'Batch data Lakehouse with a Spark MLlib recommendation engine and a RAG chatbot for semantic movie search.',
            tags: ['Airflow', 'Spark', 'Iceberg'],
            rotate: 'rotate-1',
            details: 'Airflow orchestrates a Silver/Gold pipeline: PySpark cleans and types raw MovieLens data into Iceberg tables on MinIO, then trains a collaborative-filtering ALS model with temporal decay (prioritizing recent ratings) and a blockbuster penalty to surface long-tail titles. A second pipeline enriches movies via the TMDb API, generates embeddings with Ollama (nomic-embed-text), and stores them in Postgres via pgvector for cosine-similarity search. Both are served through a Streamlit app with a recommendations tab and a RAG chatbot tab.',
            links: [
                {label: 'repository', href: 'https://github.com/luccappaz/MovieLens'}
            ]
        },
        {
            title: 'Finstream — Real-Time Credit Risk Pipeline',
            description: 'Real-time streaming pipeline for credit risk scoring, combining async local LLM inference with a Paimon Lakehouse.',
            tags: ['Flink', 'Kafka', 'Paimon'],
            rotate: '-rotate-1',
            details: 'Simulates a bank underwriting pipeline over the German Credit Data dataset: Kafka streams applications into a PyFlink job that calls a local LLM (Ollama, qwen2.5-coder) through AsyncDataStream, so risk inference never blocks the cluster\'s throughput. The model returns a structured JSON verdict (risk class + justification), which is sunk into Apache Paimon on MinIO (S3) with time-travel snapshots. Kafka consumer lag, async queue size, and JVM heap are monitored in real time via Prometheus.',
            links: [
                {label: 'repository', href: 'https://github.com/luccappaz/Finstream'}
            ]
        },
        {
            title: 'Seq2Seq with Attention — Date Normalization',
            description: 'A Sequence-to-Sequence network with Bahdanau Attention, built from scratch in TensorFlow, to convert free-form dates into the ISO 8601 standard.',
            tags: ['TensorFlow', 'Seq2Seq', 'Attention'],
            rotate: 'rotate-1',
            details: 'Character-level implementation of a custom LSTM encoder-decoder: the encoder ignores padding tokens via tf.where, keeping hidden states unchanged, and Bahdanau attention scores for padded positions are set to -1e9 before the softmax, zeroing their influence. Optimized for GPU by encoding the dataset\'s one-hot matrices as dtype=tf.int8, cutting memory usage by 75% versus tf.int32. Training is tracked with TensorBoard, with automatic checkpointing of the 3 best-performing epochs.',
            links: [
                {label: 'repository', href: 'https://github.com/luccappaz/LSTM_Attention_Model'}
            ]
        },
        {
            title: 'FuzzyMethods — Wang-Mendel & ANFIS',
            description: 'From-scratch implementation of fuzzy inference systems (Wang-Mendel and ANFIS), validated on the XOR problem and the chaotic Narendra-Li benchmark.',
            tags: ['Python', 'Fuzzy Logic', 'ANFIS'],
            rotate: '-rotate-1',
            details: 'Wang-Mendel uses dynamic partitioning with 5 quantile-based triangular membership functions (min/q25/q50/q75/max), product t-norm, and weighted-average defuzzification, plus an extrapolation engine that searches for neighboring rules sharing N-1 antecedents via Euclidean distance whenever training data is missing. ANFIS optimizes membership-function parameters and consequent coefficients via gradient descent over a Takagi-Sugeno architecture, using minimum t-norm. Both were validated on fuzzy XOR (ANFIS outperformed Wang-Mendel) and the Narendra-Li chaotic approximation benchmark, using a 5-variable autoregressive lag window.',
            links: [
                {label: 'repository', href: 'https://github.com/luccappaz/FuzzyMethods'}
            ]
        }
    ];

    const toolGroups = [
        {
            label: 'Data Engineering',
            rotate: 'rotate-1',
            items: ['Apache Spark', 'Kafka', 'Flink', 'Airflow', 'dbt', 'Iceberg', 'Paimon', 'MinIO', 'DuckDB', 'Athena']
        },
        {
            label: 'Machine Learning',
            rotate: '-rotate-1',
            items: ['TensorFlow', 'Keras', 'Scikit-Learn', 'Neural Networks', 'Attention Mechanisms', 'RAG']
        },
        {
            label: 'Databases',
            rotate: 'rotate-1',
            items: ['PostgreSQL', 'pgvector', 'SQL']
        },
        {
            label: 'Infra & Languages',
            rotate: '-rotate-1',
            items: ['Python', 'Julia', 'Bash', 'Docker', 'Docker Compose', 'Prometheus', 'Git']
        }
    ];

	const certifications = [
		{
			title: 'Certified AI Practitioner',
			issuer: 'Amazon AWS',
			date: 'August 2026',
			credentialUrl: 'https://cp.certmetrics.com/amazon/en/public/verify/credential/f0666f2c5439407385cd36ae43bd6bcc',
			tags: ['AWS', 'Amazon Bedrock', 'Amazon SageMaker AI', 'AI Agents']
		},
	];


    let expandedTitle = $state<string | null>(null);
    let status = "OPEN TO WORK";

</script>

<section
        id="home"
        class="min-h-[90vh] flex items-center scroll-mt-24"
>
    <div class="grid lg:grid-cols-[1.2fr_0.8fr] gap-16 items-center w-full">
        <div>
            <p
                    class="font-mono text-primary-400 uppercase tracking-[0.35em] text-sm mb-6"
            >
                DATA ENGINEER
            </p>

            <h1
                    class="cyber-title text-6xl md:text-8xl leading-none"
            >
                LUCCA
                <br/>
                <span class="text-primary-400">
    					DA PAZ_
    				</span>
            </h1>
            <p
                    class="mt-10 max-w-xl text-lg leading-8 text-text-300"
            >
                Building scalable data platforms,
                distributed systems and machine learning
                applications for real-time analytics.
            </p>

            <div
                    class="mt-10 space-y-3 font-mono text-primary-400"
            >
                <div>&gt; Apache Flink</div>
                <div>&gt; Apache Spark</div>
                <div>&gt; Kafka Streaming</div>
                <div>&gt; Machine Learning</div>
                <div>&gt; Lakehouse Architectures</div>
            </div>

            <div
                    class="flex flex-wrap gap-4 mt-12"
            >
                <a
                        href="#projects"
                        class="cyber-button"
                >
                    VIEW PROJECTS
                </a>
                <a
                        href="#contact"
                        class="cyber-button"
                >
                    CONTACT
                </a>
            </div>
        </div>

        <!-- RIGHT -->
        <div
                class="flex justify-center"
        >
            <div class="relative">
                <div
                        class="absolute inset-0 rounded-xl bg-primary-400/20 blur-3xl"
                ></div>
                <img
                        src="/profile.png"
                        alt="Lucca"
                        class="relative w-72 lg:w-80 rounded-xl border border-primary-400/30 glow object-cover"
                />
                <div
                        class="absolute -bottom-5 left-5 cyber-card px-5 py-3"
                >
                    <p
                            class="font-mono text-[10px] tracking-[0.25em] text-text-300"
                    >
                        STATUS
                    </p>
                    <p
                            class="font-mono text-green-400 text-sm mt-1"
                    >
                        {status}
                    </p>
                </div>
            </div>
        </div>
    </div>


</section>

<section
        id="projects"
        class="py-24 scroll-mt-24"
>
    <div class="flex items-end justify-between mb-12">
        <div>
            <p
                    class="font-mono text-xs tracking-[0.3em] uppercase text-primary-400 mb-3"
            >
                DEPLOYED PROJECTS
            </p>
            <h2 class="cyber-title text-4xl">

                PROJECTS
            </h2>
        </div>

        <div
                class="hidden md:block font-mono text-xs text-text-300"
        >
            {projects.length} PROJECTS
        </div>
    </div>

    <div class="space-y-4">
        {#each projects as project (project.title)}
            {@const isOpen = expandedTitle === project.title}

            <article class="cyber-card overflow-hidden">

                <button
                        type="button"
                        class="w-full p-6 text-left cursor-pointer"
                        onclick={() => expandedTitle = isOpen ? null : project.title}
                        aria-expanded={isOpen}
                >
                    <div class="flex justify-between items-center">

                        <div>
                            <div class="flex items-center gap-3 mb-2">
								<span class="text-primary-400 font-mono text-sm">
									{isOpen ? "−" : "+"}
								</span>

                                <h3 class="cyber-title text-xl">
                                    {project.title}
                                </h3>
                            </div>

                            <p class="text-text-300">
                                {project.description}
                            </p>
                        </div>
                    </div>
                </button>


                {#if isOpen}
                    <div
                            transition:slide={{ duration: 300 }}
                            class="border-t border-primary-400/20 p-6"
                    >

                        <p class="text-text-300 leading-8">
                            {project.details}
                        </p>


                        <div class="flex flex-wrap gap-3 mt-6">

                            {#each project.tags as tag (tag)}
								<span class="cyber-tag">
									&lt;{tag}/&gt;
								</span>
                            {/each}

                        </div>


                        <div class="flex gap-4 mt-6">

                            {#each project.links as link (link.label)}
                                <a
                                        href={link.href}
                                        target="_blank"
                                        rel="external noopener noreferrer"
                                        class="cyber-button"
                                >
                                    &gt; {link.label.toUpperCase()}
                                </a>
                            {/each}

                        </div>

                    </div>
                {/if}

            </article>

        {/each}
    </div>
</section>

<section
        id="stack"
        class="py-24 scroll-mt-24"
>
    <p class="font-mono text-xs uppercase tracking-[0.3em] text-primary-400 mb-3">
        TECHNOLOGIES
    </p>
    <h2 class="cyber-title text-4xl mb-12">
        TECH STACK
    </h2>
    <div class="grid md:grid-cols-2 gap-8">
        {#each toolGroups as group (group.label)}
            <div class="cyber-card p-7">
                <div class="flex items-center justify-between mb-6">
                    <h3 class="cyber-title text-xl">
                        {group.label}
                    </h3>
                </div>
                <div class="flex flex-wrap gap-3">
                    {#each group.items as item (item)}
						<span class="cyber-tag">
							{item}
						</span>
                    {/each}
                </div>
            </div>
        {/each}
    </div>
</section>

<section id="certifications" class="py-24 scroll-mt-24">
	<p class="font-mono text-xs uppercase tracking-[0.3em] text-primary-400 mb-3">
		QUALIFICATIONS
	</p>
	<h2 class="cyber-title text-4xl mb-12">
		CERTIFICATIONS
	</h2>

	<div class="space-y-4">
		{#each certifications as cert (cert.title)}
			<article class="cyber-card p-6">
				<div class="flex flex-col md:flex-row justify-between md:items-center gap-4">
					<div>
						<h3 class="cyber-title text-xl mb-2">
							{cert.title}
						</h3>
						<p class="text-text-300 text-sm mb-3">
							<span class="text-primary-400 font-mono">{cert.issuer}</span>
							<span class="text-text-400"> • {cert.date}</span>
						</p>
						<div class="flex flex-wrap gap-2">
							{#each cert.tags as tag (tag)}
                                <span class="cyber-tag text-xs">
                                    {tag}
                                </span>
							{/each}
						</div>
					</div>
					{#if cert.credentialUrl}

						<a href={cert.credentialUrl}
						target="_blank"
						rel="external noopener noreferrer"
						class="cyber-button whitespace-nowrap"
						>
						&gt; VIEW CREDENTIAL
						</a>
					{/if}
				</div>
			</article>
		{/each}
	</div>
</section>

<section id="about" class="py-24 scroll-mt-24">
    <p class="font-mono text-xs uppercase tracking-[0.3em] text-primary-400 mb-3">
        PROFILE
    </p>

    <h2 class="cyber-title text-4xl mb-12">
        ABOUT
    </h2>

    <div class="cyber-card p-8">
        <div class="flex flex-col lg:flex-row gap-8 items-start">

            <!-- Vídeo -->
            <div class="lg:w-1/3 flex justify-center">
                <video
                        src="/pitch.mp4"
                        controls
                        preload="metadata"
                        class="w-full rounded-xl border border-primary-400/30 glow"
                >
                    <track kind="captions" src="/pitch.vtt" srclang="en" label="English" default/>
                    <track kind="captions" src="/pitch-pt.vtt" srclang="pt" label="Portuguese"/>
                </video>
            </div>

            <!-- Conteúdo -->
            <div class="lg:w-2/3">
                <div class="grid lg:grid-cols-3 gap-10">
                    <div>
                        <p class="text-primary-400 font-mono text-xs mb-2">ROLE</p>
                        <p class="text-xl">Data Engineer</p>
                    </div>

                    <div>
                        <p class="text-primary-400 font-mono text-xs mb-2">EDUCATION</p>
                        <p>B.Sc Mathematics</p>
                        <p>M.Sc Applied Mathematics</p>
                    </div>

                    <div>
                        <p class="text-primary-400 font-mono text-xs mb-2">STATUS</p>
                        <p class="text-green-400">{status}</p>
                    </div>
                </div>

                <p class="mt-10 leading-8 text-text-300">
                    My background is in Mathematics, followed by a master's degree in
                    Applied Mathematics. I enjoy building distributed systems, modern
                    data platforms and machine learning applications, combining
                    mathematical foundations with software engineering.
                </p>
            </div>

        </div>
    </div>
</section>


<section
        id="contact"
        class="py-24 scroll-mt-24"
>
    <p class="font-mono text-xs uppercase tracking-[0.3em] text-primary-400 mb-3">
        COMMUNICATION
    </p>
    <h2 class="cyber-title text-4xl mb-12">
        CONTACT
    </h2>
    <div class="cyber-card p-8">
        <p class="text-text-300 leading-8 mb-8">

            Interested in data engineering,
            machine learning or distributed systems?
            Feel free to reach out.

        </p>
        <div class="grid md:grid-cols-3 gap-5">
            <a
                    href="mailto:luccagpaz@gmail.com"
                    class="cyber-button text-center"
            >
                luccagpaz@gmail.com
            </a>
            <a
                    href="https://github.com/luccappaz"
                    target="_blank"
                    rel="external noopener noreferrer"
                    class="cyber-button text-center"
            >
                GITHUB
            </a>
            <a
                    href="https://linkedin.com/in/luccadapaz"
                    target="_blank"
                    rel="external noopener noreferrer"
                    class="cyber-button text-center"
            >
                LINKEDIN
            </a>
        </div>
    </div>
</section>
