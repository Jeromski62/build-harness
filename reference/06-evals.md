# Module 6: Evals

## Core Idea

Hooks check individual rules. The reviewer checks individual outputs. But who checks whether the **harness as a whole** works reliably?

That's the job of evals.

A classic test checks whether code compiles. An eval checks something different: whether the harness makes the right decisions, stays consistent, and still behaves as expected after a change.

The question an eval answers:
> "Given this input, does the harness reliably produce the right output?"

## The Analogy: Test Drives

A car gets built. Hooks check whether the brakes are mounted correctly. The reviewer checks whether the steering wheel sits straight. But nobody has driven the car yet.

An eval is the test drive. A defined route, under defined conditions, checking whether the car behaves the way it should.

Swap the engine, redo the test drive. Add a new road, add a new test to the route.

## Output Eval vs. Trajectory Eval

**Output eval**
Checks the end result: is the outcome correct? Are all expected parts present? Is it complete?
Easier to write, covers most cases.

**Trajectory eval**
Checks the path: did the role search for something similar before creating something new? Was an existence check done before building?
Harder, but important. A correct result reached the wrong way is a ticking time bomb.

**For a start:** begin with output evals. Trajectory evals come once the harness is running.

## Anatomy of an Eval

Every eval has three parts:

**1. Input:** what does the harness receive? A concrete, realistic task, the way it would actually be sent in real operation.
**2. Expected Output:** what should come out? Not a vague description, concrete, checkable criteria.
**3. Rubric:** how is it scored? Pass/fail per criterion, no overall score without individual evaluation.

## Deriving Eval Categories

Categories follow from the roles (Module 2) and the pipeline (Module 4), not from a fixed list:

**1. Role evals:** one category per creator role, checks whether that role works correctly
**2. Pipeline evals:** checks whether the whole assembly line runs correctly (order, handoffs, FAIL to correction)
**3. Edge-case evals:** checks whether unexpected or borderline inputs are handled correctly

A worked example, as a pattern for your own eval:

```
Category: Role eval for "<creator role>"
Input:    "<a realistic task from the intake>"
Expected:
  ✓ Role first checks whether something similar already exists
  ✓ Output follows the naming/structure convention from AGENTS.md
  ✓ None of the hard rules from AGENTS.md are violated
  ✓ Reviewer gives PASS

Category: Edge-case eval
Input:    "<an input that would violate a hard rule>"
Expected: ✓ Role refuses or asks, instead of breaking the rule
```

The actual evals emerge from conversation about the real roles, rules, and examples of this harness, not from copying the example above.

## The Eval Set

An eval set is a collection of evals that together cover the harness.

**10-15 evals** is a good starting point: enough to catch regressions, not so many that they never actually get run.

Rule of thumb: 3-4 per creator role, 2-3 pipeline evals, 3-4 edge-case evals.

## When Evals Get Run

- **After every model update:** a new model can behave differently
- **After every harness change:** new rule, new hook, changed skill file
- **On unexpected behavior:** the agent does something unexpected, so write an eval, reproduce it, fix it

Evals aren't a one-time setup. They grow with the harness.

## Questions For You

- Which behaviors of the harness are non-negotiable?
- What would be a clear sign the harness has stopped working correctly?
- For each role: what's a realistic test case, and what's the expected result?

## What Gets Built

`harness/evals/eval-set.md`: 10-15 evals across the three categories, each with input, expected output, and rubric.

## Notes For Running This

- Best to start with the evals most rooted in a real, already-experienced failure; those are more concrete than invented ones.
- If an eval is hard to formulate, that's often a sign the underlying rule in `AGENTS.md` is still too vague. A quick trip back to Module 1 usually helps.
