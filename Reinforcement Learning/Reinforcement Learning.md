# Concept
Reinforcement learning is learning what to do—how to map situations to actions—so
as to maximize a numerical reward signal. The learner is not told which actions to
take, but instead must discover which actions yield the most reward by trying them. In
the most interesting and challenging cases, actions may affect not only the immediate reward but also the next situation and, through that, all subsequent rewards. These two
characteristics—trial-and-error search and delayed reward—are the two most important
distinguishing features of reinforcement learning.

The basic idea
is simply to capture the most important aspects of the real problem facing a learning
agent interacting over time with its environment to achieve a goal. A learning agent
must be able to sense the state of its environment to some extent and must be able to
take actions that affect the state. The agent also must have a goal or goals relating to
the state of the environment. Markov decision processes are intended to include just
these three aspects—sensation, action, and goal—in their simplest possible forms without
trivializing any of them. Any method that is well suited to solving such problems we
consider to be a reinforcement learning method.