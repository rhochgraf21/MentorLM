# We Need Mentors, Not Oracles: Exploratory Experiments In Building Effective Teaching Assistant LLMs

## Overview

How can cognitive psychology inspire the design of effective Large Language Model (LLM) tutors?

## Artifacts

This repository contains:

- [Presentation slides](presentation/Mentors_Not_Oracles.pdf), as presented at the [2024 RIT Center for Teaching and Learning AI Symposium](www.rit.edu/teaching/summer-institute).
- [4 sample tutor prompts](prompts/) used during the live demo (mildly annotated).

## Motivation

LLMs often act as knowledgeable *oracles* that provide direct answers, rather than mentors that guide students' learning of a concept. Unfortunately, this behavior often [lingers](example_conversations/simple_prompt_conversation.md) even if students explicitly request tutoring. This often leads LLMs to "search-engine" style output that provides quick overviews of content as opposed to interactive learning guidance.

"Search-engine" style outputs typically require only shallow cognitive processing from the student. This approach may be a detriment to student learning, as cognitive psychology literature has consistently found that [depth-of-processing](https://psycnet.apa.org/record/1976-00185-001) has a strong positive effect on learning outcomes.

Furthermore, these outputs rarely utilize practices from cognitive psychology that have been shown to promote student learning, such as [retrieval practice](https://psycnet.apa.org/record/2023-03242-001) or [elaborative encoding](https://psycnet.apa.org/record/1982-27279-001). Implementing these techniques into LLM tutors may benefit student learning outcomes.

## Proposed Techniques:

### Counter Prompting

To help counteract the effect of LLM preparation for chatbot setings (eg, system prompts or instruction tuning), we experiment with using prompt snippets such as the following.

```sh
You are NOT a helpful chatbot. 
You never act like a helpful chatbot.
You are <a mentor that helps people learn subjects of their choice.>
<...>
```

The fundamental idea is to "rebel" within the prompt against LLM predisposition to answer like a helpful chatbot. In subjective experience, counter-prompting appeared to help the model later instructions that steer the model to guide students or prepare it to avoid revealing an answer to the student. A benefit of this approach as compared to "jailbreak" prompts is that it uses relatively few tokens while still encouraging the LLM to more directly followed its assigned role.

### Applying Cognitive Psychology To Mentor Design

Cognitive psychology provides a variety of techniques that have been shown to improve learning quality for students. This work attempts two approaches to encourage LLMs to apply these techniques in their teaching. 

1) Providing the LLM with a description of the psychology techniques ([B](prompts/mentor_B_psychology_informed.md), [C](prompts/mentor_C_game_based_learning.md)) and asking it to apply these throughout its learning.

For example, 
```sh
- Metacognitive Skills: Teach the student about different learning strategies and encourage self-reflection on their learning process. Offer tools or methods for the student to track their own progress.

- Feedback and Scaffolding: Provide immediate, specific feedback on the student's responses. Gradually reduce the level of guidance as the student shows signs of mastery, encouraging independence.
```

2) Providing the LLM with an explicit workflow that demonstrates how it should apply cognitive psychology principles ([D](prompts/mentor_D_explicit_workflow.md)).

For example,

```sh
Your interactions with students/users should follow the outline of the steps below.

<...>

Mentor: {Provides feedback on the student’s approach and solution, kindly highlighting both strengths and areas for improvement. Then, introduces a more difficult problem tailored to the student's level, asking the student to apply the learned strategy to this new context. Encourages the student, reinforcing that they can use similar thinking.}

Student: {Applies the newly learned strategies to the original problem.}

Mentor: {Observes and provides feedback on the student's approach to this more difficult problem, ensuring not to solve it for them but to help them think through it. Introduce another related problem if necessary to practice spaced repetition and interleaved practice, helping solidify the concept further.}

Student: {Engages with the new problem, applying learned concepts and strategies.}
```

Providing an explicit conversation flow as in the above prompt allows the developer to more directly design the tutor. This places additional burden on the developer to consider cognitive psychology principles during agent design (if desired). While more burdensome for the developer, this does allow for advanced high-level design using cognitive psychology principles. For example, the developer can design the LLM flow to maximize student engagement (attention/rehearsal) by designing a conversation flow where the LLM agent pauses and waits for student responses to retrieval questions, encouraging students to engage with the questions.

For more information on applying cognitive psychology principles to LLM tutors, see slides 8-14 from the [presentation slides](presentation/Mentors_Not_Oracles.pdf).

## The Prompts

- [Mentor A](prompts/mentor_A_baseline.md): A baseline mentor without any cognitive psychology principles or counter-prompting.
- [Mentor B](prompts/mentor_B_psychology_informed.md): A mentor that is prompted with several cognitive psychology principles, but is not explicitly given a workflow to apply these principles. Does not implement counter-prompting.
- [Mentor C](prompts/mentor_C_game_based_learning.md): A mentor that is prompted with several cognitive psychology principles in a game based setting, but is not given an explicit workflow for the games or the psychology principles. Utilizes counter-prompting.
- [Mentor D](prompts/mentor_D_explicit_workflow.md): A mentor that is given an explicit (zero-shot) workflow for applying cognitive psychology principles to its teaching. Utilizes counter-prompting.

## Subjective Evaluation

In this section, subjective interpretations of model performance of the mentors are discussed. This includes both creator assessment and responses from live demonstration. In the live demonstration, participants were provided with a one-line description of the four prompts and asked to guess which prompt powered each of four single-blinded tutors by interacting with each.

- [Mentor A](prompts/mentor_A_baseline.md): During evaluation, Mentor A is immediately differentiated from the other mentors. Participants immediately noticed that its responses were summary in nature. Mentor A is effective in discussing a large portion of material, however both the depth of the content and learner engagement with its material is limited.

- [Mentor B](prompts/mentor_B_psychology_informed.md): In live evaluation, Mentor B was often confused with Mentor D. Mentor B was noted for its use of cognitive psychology principles, particularly active recall questions, and for its use of more "lecture" style content, as compared to Mentor D's demonstrative examples and elaborative reasoning questions. The mentor tends to use clear and simple language, a direct result of the prompt's final paragraph.

- [Mentor C](prompts/mentor_C_game_based_learning.md): In evaluation, Mentor C was often spotted quickly due to its gamified focus. While game-based approaches are sometimes effective as demonstrative examples, the complexity of generating games occasionally added additional complexity to the model's responses in comparison to Mentor B or D, making understanding the underlying content more difficult. 

- [Mentor D](prompts/mentor_D_explicit_workflow.md): This mentor was described as the most *insightful* mentor, competently using concrete examples to demonstrate intuition behind concepts. It was found to be rather engaging, with consistent pauses encouraging learner engagement with the material. However, this mentor has a pervasive tendency to get stuck in a cycle of asking students elaborative interrogation questions. It also tends to deliver material more slowly than that of the other mentors.

## Models Used

`gpt-4-turbo-2024-04-09`

## License

[MIT License](LICENSE).