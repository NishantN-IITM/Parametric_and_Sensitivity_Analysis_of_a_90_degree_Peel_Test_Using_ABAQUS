# Abaqus Model Implementation

The 90° peel test was implemented in Abaqus using a surface-based cohesive zone model. The following excerpts from the Abaqus input deck illustrate the key aspects of the numerical formulation.

## Material Definition

### Adherend

```abaqus
*Material, name=Adherend
*Elastic
70000., 0.3
```

### Adhesive

```abaqus
*Material, name=Adhesive
*Density
0.001
*Elastic
990., 0.33
```

---

## Cohesive Zone Model

The adhesive interface was represented using a surface-based cohesive interaction with damage initiation and damage evolution.

### Damage Initiation

```abaqus
*Damage Initiation, criterion=MAXS
0.099, 0.99, 99.
```

### Damage Evolution

```abaqus
*Damage Evolution, type=ENERGY,
mixed mode behavior=BK, power=1.75
0.154, 0.22, 0.22
```

### Damage Stabilization

```abaqus
*Damage Stabilization
0.001
```

The mixed-mode fracture response was governed by the Benzeggagh–Kenane (BK) criterion.

---

## Contact Definition

The interface interaction between the thin film and rigid substrate was defined using a surface-to-surface contact pair.

```abaqus
*Contact Pair, interaction=IntProp-1,
small sliding,
type=SURFACE TO SURFACE
s_Surf-1, m_Surf-1
```

---

## Boundary Conditions

### Symmetry Condition

```abaqus
*Boundary
Set-4, YSYMM
```

### Applied Peel Displacement

```abaqus
*Boundary
Set-3, 1, 1
Set-3, 2, 2, 50.
Set-3, 6, 6
```

A vertical displacement of 50 mm was prescribed at the loading point to simulate the peeling process.

---

## Dynamic Implicit Analysis Step

```abaqus
*Step, name=Step-1, nlgeom=YES, inc=10000

*Dynamic, application=QUASI-STATIC,
initial=NO
1e-05,0.1,1e-15
```

Geometric nonlinearity was enabled to capture the large rotations and deformation associated with the peel test.

---

## Output Requests

The reaction force and displacement at the loading point were extracted to generate the force–displacement curves.

```abaqus
*Output, history

*Node Output, nset=LOAD-POINT
RF2, U2
```

where:

- RF2 = Reaction Force
- U2 = Vertical Displacement


# Results and Model Optimization

The 90° peel-test model was systematically refined through a series of numerical investigations aimed at obtaining a stable and physically realistic force–displacement response. The study focused on understanding how different modeling assumptions and numerical procedures influence the predicted peeling behavior and interfacial debonding response.

The following aspects were investigated:

- Analysis procedure selection
  - Static General
  - Dynamic Implicit
  - Dynamic Explicit

- Boundary condition sensitivity

- Cohesive damage parameters
  - Cohesive strength (σc)
  - Fracture energy (Gc)
  - Damage stabilization parameters

- Contact and interaction definitions

- Adhesive geometry variations

The final model configuration was selected based on numerical stability, convergence behavior, computational efficiency, and agreement with the expected peel-force response.

---

## Solver Comparison

Different Abaqus solution procedures were evaluated to assess their influence on convergence, numerical stability, and the resulting force–displacement response.


### Dynamic Implicit

The dynamic implicit procedure provided improved convergence while maintaining a quasi-static response. This approach was found to be more robust for simulating progressive interfacial debonding.

<img width="464" height="274" alt="image" src="https://github.com/user-attachments/assets/63b76c96-94f1-4cf5-bf06-42d2246b529d" />


### Dynamic Explicit

The dynamic explicit formulation offered excellent numerical stability and successfully captured the complete peeling process. However, appropriate loading rates and mass scaling considerations were required to ensure quasi-static conditions.

<img width="478" height="274" alt="image" src="https://github.com/user-attachments/assets/f839f2ed-4f7d-4826-8be9-d32e5dacfc99" />


### Static General

The static general procedure exhibited convergence difficulties during crack initiation and propagation due to severe nonlinearities associated with interface damage evolution.

<img width="481" height="268" alt="image" src="https://github.com/user-attachments/assets/b294d232-0a18-4886-8473-5a3b8b5ce2fa" />


---

## Boundary Condition Investigation

Multiple boundary-condition configurations were examined to understand their influence on global compliance, peel-force response, and deformation characteristics of the thin-film system.

The study included variations in:

- Loading constraints
- Symmetry conditions
- Reference-point coupling definitions
- Kinematic constraints

<img width="886" height="299" alt="image" src="https://github.com/user-attachments/assets/30d61237-0e9a-47bb-a9c2-84fbc661ded7" />
<img width="885" height="286" alt="image" src="https://github.com/user-attachments/assets/7ff8e5b6-6705-4917-9454-8717deb9a3c8" />
<img width="881" height="257" alt="image" src="https://github.com/user-attachments/assets/b6a30ae8-69aa-4691-88c7-0046ad70384a" />

---

## Contact and Interaction Studies

Several interaction formulations were investigated to evaluate their influence on interface separation and damage evolution.

The following aspects were considered:

- Surface-to-surface contact definitions
- Cohesive interaction properties
- Contact stiffness parameters
- Damage stabilization parameters

These studies were essential in obtaining a stable and physically meaningful debonding response.

<img width="884" height="304" alt="image" src="https://github.com/user-attachments/assets/35c288e6-c888-45b9-8d6c-4b19a21a05ec" />
<img width="885" height="244" alt="image" src="https://github.com/user-attachments/assets/009f0581-4eda-47ff-b22c-dec6d7ece0f9" />
<img width="875" height="235" alt="image" src="https://github.com/user-attachments/assets/e3bd52b5-d71f-4b4d-b92c-134eb2ab6442" />
<img width="885" height="246" alt="image" src="https://github.com/user-attachments/assets/2e15ebcf-de54-43e8-b8e9-0ec1f818d44b" />


---

## Cohesive Parameter Calibration

The cohesive properties were iteratively adjusted to investigate their effect on damage initiation, crack propagation, and peel-force evolution.

Parameters considered include:

- Cohesive Strength (σc)
- Fracture Energy (Gc)
- Damage Stabilization Coefficient

The calibration process enabled identification of parameter combinations that produced realistic force–displacement characteristics while maintaining numerical stability.


---

## Adhesive Geometry Investigation

The influence of adhesive length on the peeling response was also examined.

Different adhesive lengths were considered to evaluate:

- Debonding progression
- Energy dissipation
- Peel-force characteristics


---


## Final Optimized Model

The optimized model was obtained after systematically evaluating solver selection, boundary conditions, cohesive parameters, contact definitions, and adhesive geometry.

The final configuration demonstrated:

- Stable convergence
- Physically realistic debonding behavior
- Consistent force–displacement response
- Improved numerical robustness

The resulting force–displacement curve is shown below.

<img width="547" height="312" alt="image" src="https://github.com/user-attachments/assets/4e8d5d60-8463-4fed-b491-45a913ba14e1" />
<img width="646" height="159" alt="image" src="https://github.com/user-attachments/assets/20eed778-d7b1-451a-b5d2-14eebf329eed" />


---

## Key Findings

- Found that if the thickness of the adhesive is considerably high then you can go with the Cohesive Element approach but for negligible thickness adhesive Surface based contact works just fine.
- Static General approach procedure provided improved robustness compared to the Dynamic Implicit and Dynamic Explicit.
- Boundary conditions significantly influenced the compliance and force response of the peel-test system.
- Cohesive strength (σc) primarily affected damage initiation and peak force.
- Fracture energy (Gc) governed crack propagation and steady-state peeling behavior.
- Contact definitions and damage stabilization parameters played a crucial role in numerical convergence.
- Adhesive geometry influenced the overall debonding response and energy dissipation.
- A carefully calibrated cohesive-zone model was necessary to obtain physically realistic peel-force predictions.
