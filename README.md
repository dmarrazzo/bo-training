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

lab02 has all the annotations

