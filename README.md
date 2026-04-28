# JARCHAIN Lazy Pre-decode

Implementation of lazy pre-decode optimization for JAVM interpreter.

## Overview

This implementation adds optional lazy pre-decode mode to the JAVM interpreter. Instead of pre-decoding all instructions upfront, basic blocks are decoded on-demand during execution.

## Files

- `src/lazy_predecode.rs` - Core implementation
- `benches/lazy_predecode.rs` - Benchmark suite
- `PR_PROPOSAL.md` - Detailed PR proposal

## Integration

To integrate with the main JAR repository:

1. Copy `src/lazy_predecode.rs` to `grey/crates/javm/src/interpreter/`
2. Add new fields to `InterpreterProgram` in `grey/crates/javm/src/backend.rs`
3. Modify `run_segment()` to use `get_decoded()` instead of direct access
4. Add `lazy-predecode` feature to `grey/crates/javm/Cargo.toml`

## Running Tests

```bash
# Run unit tests
cargo test -p javm

# Run benchmarks
cargo bench --bench lazy_predecode
```

## Environment Variables

| Variable | Values | Default | Description |
|----------|--------|---------|-------------|
| `GREY_PVM_LAZY` | `true`, `false` | `true` | Enable/disable lazy mode |

## Performance

Expected improvements:

| Program Type | Improvement |
|--------------|-------------|
| Short (<10 blocks) | ~30-40% |
| Medium (10-50 blocks) | ~15-20% |
| Long (>50% coverage) | Auto-switch to eager |

## Related PRs

- #812: `feed_gas_direct()` optimization
- #813: `feed_direct()` optimization
- #810: `unlikely()` hint optimization
- #400: javm interpreter performance tracking issue
