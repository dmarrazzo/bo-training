# Business Optimizer Training labs

## lab01

### Goal

Practice the annotations

### Instruction

When you find the planning entity, add the property `difficultyComparatorClass` that is used to initialize the heuristic construction.

    @PlanningEntity(difficultyComparatorClass = CloudProcessDifficultyComparator.class)

When you find the planning variable, add the property `strengthComparatorClass` that is used to initialize the heuristic construction.

    @PlanningVariable(valueRangeProviderRefs = {"..."},
            strengthComparatorClass = CloudComputerStrengthComparator.class)

Spot the planning solution and add:

    @PlanningSolution

Other annotations for the solution:

    @PlanningScore
    @ValueRangeProvider(id = "computerRange")
    @ProblemFactCollectionProperty
    @PlanningEntityCollectionProperty

### Solution

Check lab02 model that has all the annotations

## lab02

### Goal

Practices constriaints defintitions with DRL

### Instruction

Open `lab02/src/main/resources/org/optaplanner/examples/cloudbalancing/solver/cloudBalancingScoreRules.drl`

Add the following rules:

- No more than 4 processes per computer
- Distribute network bandwidth fairly

Hints:

- Write one rule per time and test it
- Before implementing the 3rd rule read in the documentation **Fairness Score Constraints** in the **score trap** section

### Solution

#### No more than 4 processes per computer

```js
rule "atMost4ProcessesPerComputer"
    when
    $computer : CloudComputer()
    accumulate(
        $c : CloudProcess(computer == $computer);
        $count : count($c);
        $count > 4
    )
    then
        scoreHolder.addHardConstraintMatch(kcontext, 4 - $count.intValue());
end
```

#### Pitfalls

A computer that has 6 processes is worse than a computer that has 5 processes.
The score function should reflect that, even if the customer's business analyst does not want to think
about any solution for which a computer has more than 4 computers (because it's an infeasible solution).

This code might trigger a http://docs.optaplanner.org/latest/optaplanner-docs/html_single/index.html#scoreTrap

```js
rule "atMost4ProcessesPerComputer"
when
    ...
then
    scoreHolder.addHardConstraintMatch(kcontext, -1); // BAD: score trap
end
```

#### Distribute network bandwidth fairly

```js
rule "distributeNetworkBandwidthFairly"
    when
        $computer : CloudComputer()
        accumulate(
            CloudProcess(
                computer == $computer,
                $requiredNetworkBandwidth : requiredNetworkBandwidth);
            $total : sum($requiredNetworkBandwidth)
        )
    then
        scoreHolder.addSoftConstraintMatch(kcontext, - ($total * $total));
end
```

## lab03

### Goal

Practice the benchmark

### Instruction

Open: `lab03/src/main/resources/logback.xml`:

- change level to `info`

Work in lab03 project where you will find a clean project.
Search the benchmark class: `CloudBalancingBenchmarkHelloWorld`.

Open the benchmark configuration file used in the:
`lab03/src/main/resources/org/optaplanner/examples/cloudbalancing/optional/benchmark/cloudBalancingBenchmarkConfig.xml`


There are 4 sections:

- `problemBenchmarks` data sets used for the benchmark
- `solver` here it's possible to reduce the termination time
- `solverBenchmark` 5 different solver configuration

Run the benchmarker reducing the termination time to 2 minutes:

- `(2 minutes * 3 configuration * 5 data sets) / parallel threads`, in a laptop with 4 cores, leads to 17 minutes of wait time

To get a more predictable result, configure the number of thread to fixed value.
Which is the property to set it up?

Explore the report.

Optionally, try the other configurations available.