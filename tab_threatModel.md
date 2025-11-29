---
title: Threat model
layout:  null
tab: true
order: 5
tags: tm-tag
---

## Threat Model DSL & Examples (`threatModel_Template/threat_model.md`)

This framework leverages PyTM's modeling primitives. For a comprehensive reference of all available attributes and their usage, please refer to the [PyTM Documentation](https://owasp.github.io/PyTM/Threat-Model-as-Code/). Note that this framework may extend PyTM with additional attributes or behaviors.

```markdown
# Threat Model: Advanced DMZ Architecture

## Description
A network with a DMZ, external/internal firewalls, and a command zone. The goal is to identify STRIDE threats and map them to MITRE ATT&CK.

## Boundaries
- **Internet**: color=lightcoral
- **DMZ**: color=khaki
- **Intranet**: color=lightgreen
- **Command Zone**: color=lightsteelblue

## Actors
- **External Client 1**: boundary=Internet
- **Operator**: boundary=Command Zone

## Servers
- **External Firewall**: boundary=DMZ
- **Internal Firewall**: boundary=Intranet
- **Central Server**: boundary=Intranet

## Data
- **Web Traffic**: classification=public, lifetime=transient

## Dataflows
- **External Client to External Firewall**: from="External Client 1", to="External Firewall", protocol="HTTPS", data="Web Traffic", is_encrypted=True

## Severity Multipliers
- **Central Server**: 1.5
- **External Firewall**: 2.0

## Custom Mitre Mapping
- **Protocol Tampering**: tactics=["Impact", "Defense Evasion"], techniques=[{"id": "T1565", "name": "Data Manipulation"}]
```

### Common Attributes

Several elements in the threat model DSL support common attributes to enhance their representation and behavior:

-   **`color`**: Specifies the color of the element in the generated diagrams.
    -   **Applies to**: Boundaries, Actors, Dataflows, Protocol Styles.
    -   **Values**: Can be a standard HTML color name (e.g., `red`, `blue`, `lightgray`), or a hexadecimal color code (e.g., `#FF0000`).

-   **`line_style`**: Defines the style of lines for dataflows or protocols in diagrams.
    -   **Applies to**: Protocol Styles.
    -   **Values**: `solid`, `dashed`, `dotted`.

-   **`is_trusted`**: Indicates whether a Boundary is considered trusted. This can influence threat analysis.
    -   **Applies to**: Boundaries.
    -   **Values**: `True` or `False`.

-   **`is_encrypted`**: Specifies whether a Dataflow is encrypted. This is crucial for information disclosure threat analysis.
    -   **Applies to**: Dataflows.
    -   **Values**: `True` or `False`.

### Protocol Styles and Legends

To ensure that protocols are correctly styled in diagrams and appear in the legend, you must define them in the `## Protocol Styles` section of your threat model. The system **intentionally does not** assign default colors to new protocols. This gives you full control over the final visualization.

**How it works:**

1.  **Use a protocol** in a `Dataflow`, e.g., `protocol="NEW_PROTO"`.
2.  **Define its style** under `## Protocol Styles` to make it appear in the legend:
    ```markdown
    ## Protocol Styles
    - **HTTPS**: color=darkgreen, line_style=solid
    - **HTTP**: color=red, line_style=solid
    - **NEW_PROTO**: color=cyan, line_style=dotted
    ```

If you skip step 2, the protocol will be drawn in the diagram with a default style, but it will **not** be included in the legend.

### Bidirectional Dataflow Visualization

This makes bidirectional communications visually clear and reduces clutter in your architecture diagrams.

**Example:**

If your model contains:
```markdown
## Dataflows
- A to B: from="A", to="B", protocol="HTTPS"
- B to A: from="B", to="A", protocol="HTTPS"
```

The diagram will show:
```
A <--> B
```
(with a single arrow using `dir="both"` in DOT/Graphviz)

This feature is enabled by default and works for all protocols.


