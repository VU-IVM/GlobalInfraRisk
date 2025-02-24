# Maximum Damages Explained

Setting maximum damage values is often complex, as it depends on multiple factors and involves uncertainty. As a result, universal baseline values are rare, and it is strongly recommended to define these values using local context and knowledge.

### **Defining Maximum Damages**
Maximum damages should be provided in a format that aligns with the expected input for `DamageScanner()`. The input should be a **pandas DataFrame** or a **dictionary** that maps object types to their corresponding maximum damage values. Each object type must have a defined value to ensure compatibility with the `DamageScanner()`.

```{important}
Maximum damages should be defined per unique infrastructure object type. And each object type should have only one geometry type, because the maximum damage has to be defined differently for Points than for Polygons. Check out the [**Introducing `DamageScanner()`](https://vu-ivm.github.io/GlobalInfraRisk/howto/damagescanner.html) page for more information and solutions.
```

**Example:**
```python
import pandas as pd

# Defining maximum damages via a dictionary
maxdam_dict = = {'clinic' : 1000, 
               'pharmacy' : 1000,
               'hospital' : 1000,}

maxdam = pd.DataFrame.from_dict(maxdam_dict, orient='index').reset_index()
maxdam.columns = ['object_type','damage']
```

### **Alternative Metrics Beyond Monetary Damage**
In some cases, the impact of hazards may be better measured using other metrics beyond monetary losses, such as:
- **Power Generation Loss:** Impact on electricity supply from damaged power plants. To focus on power generation loss, one would most likely use the generation capacity as  the potential maximum impact metric.
- **Service Disruption:** Loss of connectivity or services (e.g., healthcare or telecommunications outages).
- **Population Impacted:** Number of people affected due to service interruptions (e.g., loss of potable water supply).

These alternative metrics are useful when the focus is on resilience or recovery rather than purely financial loss.
