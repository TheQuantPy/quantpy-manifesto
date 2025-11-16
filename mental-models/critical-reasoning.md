## Critical Reasoning — Summary

Critical reasoning is the discipline of evaluating information, arguments, and decisions with clarity, structure, and justified judgment. At its core, it focuses on how we form beliefs, assess evidence, uncover assumptions, and arrive at conclusions that are logically and empirically grounded. In quantitative fields—especially those involving models, uncertainty, and decision-making—critical reasoning acts as the framework that keeps analysis rigorous, transparent, and robust.

The value of critical reasoning is not just in identifying errors or weak arguments. Its real power lies in providing a systematic way to think: clarifying questions, separating signal from noise, and ensuring that conclusions follow from evidence rather than intuition or bias.

⸻

#### Why the Three Approaches Matter

1. Critical Thinking / Argument Analysis

This approach ensures that claims, models, and explanations are examined structurally rather than accepted at face value. By breaking arguments into premises, evidence, and conclusions, it becomes easier to detect logical gaps, hidden assumptions, or unjustified leaps. In research and quantitative finance, this skill protects against adopting flawed theories or overextending conclusions beyond what the data supports.

2. Probabilistic (Bayesian) Reasoning

Real-world problems rarely come with certainty. Bayesian reasoning provides a formal way to update beliefs as new information arrives, making it indispensable when working with risk, stochastic processes, or predictive models. It captures uncertainty explicitly, helping avoid overconfidence and enabling decisions that balance probabilities, costs, and outcomes. This makes it one of the most practical reasoning tools in scientific and quantitative contexts.

3. Model-Based / Scientific Reasoning

This approach anchors reasoning in structured modeling: defining assumptions, deriving implications, testing predictions, and refining iteratively. It mirrors the scientific method and ensures that explanations are not only coherent but also empirically validated. Whether designing a financial model or exploring a theoretical mechanism, this method ensures that conclusions are grounded in testable structure rather than speculation.

⸻

Together, these three approaches form a complementary toolkit: one checks the structure of arguments, one handles uncertainty, and one provides a disciplined way to build and evaluate models. Practiced together, they create a strong foundation for rigorous analysis, sound decision-making, and reliable knowledge-building across technical and academic domains.

#### Critical reasoning step-by-step

1) Critical thinking / argument analysis

Use when: evaluating claims, reading papers, or deciding whether a model’s conclusion is believable.

Steps
	1.	State the question — write the claim or decision in one sentence.
	2.	Map the argument — list premises, evidence, and the conclusion. (Draw a mini flow: evidence → inference → claim.)
	3.	Check definitions & scope — make sure key terms are precise and the claim’s scope (population, time, conditions) is explicit.
	4.	Evaluate evidence quality — ask: source, data quality, sample size, measurement error, biases, alternative explanations.
	5.	Look for logical gaps/fallacies — causal claims from correlation, cherry-picking, strawman, false dichotomy, etc.
	6.	Weigh alternatives — generate plausible rival explanations and see whether the evidence discriminates between them.
	7.	Form a provisional judgment — accept/reject/suspend, and state what would change your mind (needed evidence).
	8.	Document and repeat — write a short note of your reasoning; revisit when new info arrives.

Practical tip: when reading a paper, force yourself to summarize the claim + top 2 supporting items and top 2 weaknesses in 3 lines.

2) Probabilistic (Bayesian) reasoning

Use when: combining prior knowledge with data, updating beliefs, or quantifying uncertainty.

Steps
	1.	Encode prior beliefs — express your initial belief as a probability or a distribution (even a rough numeric: e.g., “I think the model is 70% likely to generalize”).
	2.	Formalize the evidence — translate data into a likelihood: how probable is the observed data under different hypotheses or parameter values?
	3.	Apply the update rule — use Bayes’ rule conceptually: Posterior ∝ Prior × Likelihood. (In practice compute numerically or approximate.)
	4.	Compute summaries — mean, median, credible intervals, or predictive distributions to answer the question you care about.
	5.	Make a decision under uncertainty — choose actions by expected utility or loss (weighing costs of type-I vs type-II errors).
	6.	Check calibration — compare posterior predictive checks with held-out data; are your credible intervals well calibrated?
	7.	Iterate — new data → repeat the update.

Finance example: start with a prior on daily volatility, observe recent return sample (likelihood), update to get a posterior volatility for risk limits or sizing trades.

Practical tip: if exact Bayes is hard, use simpler proxies (conjugate priors, bootstrapping, or approximate Bayesian computation / variational methods).

3) Model-based / scientific reasoning (hypothesis → model → test → refine)

Use when: building quantitative models, designing experiments, or solving complex engineering problems.

Steps
	1.	Define the problem and success metric — what question does the model answer and how will you measure success?
	2.	State assumptions explicitly — list simplifying assumptions and which ones you’ll relax later.
	3.	Build the minimal model — construct the simplest model that can produce the behaviors you care about (equations, algorithm, or causal graph).
	4.	Derive implications — work out explicit, testable predictions (analytic or simulated).
	5.	Test against data — compare predictions to data using plots, residual checks, held-out performance, or experiments.
	6.	Analyze failure modes — where and why does the model fail? Are assumptions violated? Which components matter most?
	7.	Refine and re-parameterize — add complexity only where it improves predictive power or explanatory value (regularize).
	8.	Communicate limitations — report where the model applies and where it doesn’t.

Practical tip: follow the 80/20 rule — get 80% of the explanatory power with a simple model, quantify remaining errors, then iterate.