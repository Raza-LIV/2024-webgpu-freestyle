# 2025 WebGPU sandbox
Small test of what this API can do. Shaders using [WGSL](https://gpuweb.github.io/gpuweb/wgsl) are really powerfull.

![Example](https://github.com/user-attachments/assets/5f90081b-a1a6-44f4-b9a3-1be3ac75a6e1)

```wgsl
@group(0) @binding(0) var<uniform> grid: vec2f;

struct VertexInput {
    @location(0) pos: vec2f,
    @builtin(instance_index) instance: u32,
};

struct VertexOutput {
    @builtin(position) pos: vec4f,
    @location(0) cell: vec2f,
};

@vertex
fn vertexMain(input: VertexInput) -> VertexOutput {
    let i = f32(input.instance);
    let cell = vec2f(i % grid.x, floor(i / grid.x));

    let cellOffset = cell / grid * 2;
    let gridPos = (input.pos + 1) / grid - 1 + cellOffset;

    var output: VertexOutput;
    output.pos = vec4f(gridPos, 0, 1);
    output.cell = cell;
    return output;
}

struct FragInput {
    @location(0) cell: vec2f,
};

@fragment
fn fragmentMain(input: FragInput) -> @location(0) vec4f {
    let c = input.cell / grid;
    return vec4f(c, 1-c.x, 1);
}
```
