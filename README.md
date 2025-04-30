# Type-1 Fuzzy Logic System

## Overview
This repository contains a comprehensive implementation of a Type-1 Fuzzy Logic System using Python and the scikit-fuzzy library. The implementation provides a complete workflow for fuzzy inference, including membership function creation, rule definition, inference engine, and defuzzification.

![Fuzzy Logic System Diagram](https://miro.medium.com/max/1400/1*Cx82V5ykR-zxnC6zgLPJqg.png)

## Features

### Core Fuzzy Logic Components
- **Flexible Membership Functions**: Support for multiple types of membership functions:
  - Triangular (`trimf`)
  - Trapezoidal (`trapmf`)
  - Gaussian (`gaussmf`)
  - Generalized Bell (`gbellmf`)
  - Double Gaussian (`gauss2mf`)
- **Comprehensive Fuzzy Inference Engine**:
  - Mamdani inference method
  - Rule mapping and activation
  - Aggregation of rule outputs
- **Defuzzification Methods**:
  - Centroid defuzzification (default)
  - Extends easily to other methods (bisector, mean of maximum, etc.)

### Visualization
- **Interactive Membership Function Plotting**: Visualize all membership functions for inputs and outputs
- **Rule Activation Visualization**: See which rules are firing and their strength
- **Aggregated Output Visualization**: Observe the combined effect of all rules

### User Interface
- **Interactive Sliders**: Adjust input values in real-time and see results immediately
- **Custom Fuzzy System Creator**: Define your own variables, membership functions, and rules

## Example Applications

### Temperature Control System
The notebook includes a complete example of a fuzzy temperature control system:
- **Inputs**:
  - Current temperature (0-40°C)
  - Temperature change rate (-5 to 5°C/min)
- **Output**:
  - Heating power (0-100%)
- **Rules**: 9 fuzzy rules connecting the inputs to the output

### Custom System Development
The notebook provides a framework for creating your own fuzzy systems for applications such as:
- Control systems
- Decision support systems
- Pattern classification
- Image processing
- Risk assessment
- And many more...

## Usage

### Installation
```python
# Install required libraries
!pip install scikit-fuzzy matplotlib numpy
```

### Basic Workflow

1. **Import Libraries**
```python
import numpy as np
import skfuzzy as fuzz
import matplotlib.pyplot as plt
```

2. **Define Universes of Discourse**
```python
# Universe for temperature: 0 to 40°C
temp_universe = np.arange(0, 41, 1)
```

3. **Create Membership Functions**
```python
# Create a triangular membership function for "Cold" temperature
cold_mf = create_membership_function(temp_universe, 'Cold', 'trimf', [0, 0, 20])
```

4. **Define Rules**
```python
# Define rules as a matrix where each element represents the output term index
rule_matrix = [
    [2, 2, 1],  # For input1_term1 and input2_terms[0,1,2]
    [2, 1, 0],  # For input1_term2 and input2_terms[0,1,2]
    [1, 0, 0]   # For input1_term3 and input2_terms[0,1,2]
]
```

5. **Perform Inference**
```python
# Get the crisp output for specific input values
result = fuzzy_inference(input1_val, input2_val, universe1, universe2, output_universe,
                       input1_mfs, input2_mfs, output_mfs, rule_matrix)
```

### Interactive Demo
The interactive demo allows you to experiment with different input values and see the results in real-time:

```python
run_interactive_demo()
```

## Implementation Details

### Core Functions

#### `fuzz_Mem_Func`
Creates membership functions of various types based on parameters.

#### `create_membership_function`
Wrapper function that creates a membership function with a name and parameters.

#### `plot_membership_functions`
Visualizes a set of membership functions on the same plot.

#### `fuzzy_inference`
The main inference function that:
1. Fuzzifies crisp input values
2. Applies fuzzy rules and calculates rule strength
3. Applies implication to determine rule outputs
4. Aggregates all rule outputs
5. Defuzzifies to produce a crisp output value

#### `create_custom_fuzzy_system`
Function to create a complete, custom fuzzy system with user-defined parameters.

## Advanced Usage

### Custom Membership Functions
```python
# Create a Gaussian membership function
gaussian_mf = create_membership_function(universe, 'Term', 'gaussmf', [mean, sigma])

# Create a Generalized Bell membership function
gbell_mf = create_membership_function(universe, 'Term', 'gbellmf', [a, b, c])
```

### Alternative Rule Formats
The implementation supports different ways to represent rules:
```python
# Rule matrix approach (default)
rule_matrix = [
    [output_idx_for_input1_term1_input2_term1, output_idx_for_input1_term1_input2_term2, ...],
    [output_idx_for_input1_term2_input2_term1, output_idx_for_input1_term2_input2_term2, ...],
    ...
]

# Alternatively, you can define rules individually
rules = [
    {'antecedent': [('input1', 'Low'), ('input2', 'High')], 'consequent': ('output', 'Medium')},
    ...
]
```

## Extending the System

### Adding New Membership Function Types
To add support for new membership function types:

```python
def fuzz_Mem_Func(var, typeOfMf, lst):
    # Existing code...
    
    elif typeOfMf == 'your_new_mf_type':
        # Implement your custom membership function
        return your_custom_mf_function(var, *lst)
```

### Supporting More Complex Rule Structures
For more complex rules (e.g., with OR operators):

```python
# Example implementation for OR in rule antecedents
def apply_or_rules(input_degrees):
    return np.max(input_degrees)  # OR is implemented as maximum
```

## Applications

### Control Systems
Fuzzy logic is ideal for control systems where precise mathematical models are difficult to derive:
- HVAC control
- Autofocus systems
- Washing machines
- Vehicle stability control

### Decision Support
Fuzzy systems can model expert decision-making:
- Medical diagnosis
- Financial analysis
- Risk assessment

### Pattern Recognition
Fuzzy logic can handle uncertainty in classification problems:
- Image recognition
- Speech recognition
- Handwriting recognition

## Performance Considerations

### Computational Efficiency
- The current implementation is suitable for educational purposes and systems with modest computational requirements
- For real-time applications, consider:
  - Pre-computing membership functions
  - Optimizing the inference process
  - Using lookup tables for common operations

### Memory Usage
- Universe size affects memory usage
- Balance between resolution and memory requirements
- For large-scale systems, consider sparse representations

## Troubleshooting

### Common Issues

1. **Empty or Incorrect Outputs**
   - Check if your membership functions overlap appropriately
   - Ensure rule matrix dimensions match input term counts
   - Verify that at least one rule activates for your inputs

2. **Unexpected Results**
   - Visualize membership functions to ensure they're defined correctly
   - Trace rule activations to understand which rules are firing
   - Examine the aggregated output to see if it makes sense

3. **Performance Issues**
   - Reduce universe resolution if processing is slow
   - Optimize membership function calculations
   - Consider fewer or simpler rules

## Resources

### Further Reading
- Zadeh, L.A. (1965). "Fuzzy sets". Information and Control, 8(3): 338–353.
- Mamdani, E.H. and Assilian, S. (1975). "An experiment in linguistic synthesis with a fuzzy logic controller". International Journal of Man-Machine Studies, 7(1): 1–13.
- Ross, T.J. (2010). "Fuzzy Logic with Engineering Applications". Wiley.

### Useful Links
- [scikit-fuzzy Documentation](https://pythonhosted.org/scikit-fuzzy/)
- [Introduction to Fuzzy Logic Control Systems](https://www.mathworks.com/help/fuzzy/foundations-of-fuzzy-logic.html)
- [Fuzzy Logic Tutorial](https://www.tutorialspoint.com/fuzzy_logic/index.htm)

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgements
- scikit-fuzzy developers for providing an excellent fuzzy logic library
- The fuzzy logic community for advancing the field

---

*"The complexity of a problem is not a license to use complex methods. The simplicity of fuzzy logic comes from the fact that it mimics how humans think." - Lotfi Zadeh*
