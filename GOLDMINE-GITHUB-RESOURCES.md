# Goldmine GitHub Resources (Curated)

This is a high-signal list for your **From CPU to Browser** path.

How to use:
- Pick only 2-3 repos per week at first.
- Read architecture/docs first, then source code.
- Build/run one repo every week.

---

## 0) Absolute Top 12 (Start Here)

1. https://github.com/nand2tetris/projects
2. https://github.com/mit-pdos/xv6-riscv
3. https://github.com/beejjorgensen/bgnet
4. https://github.com/the-tcpdump-group/tcpdump
5. https://github.com/servo/servo
6. https://github.com/LadybirdBrowser/ladybird
7. https://github.com/v8/v8
8. https://github.com/bellard/quickjs
9. https://github.com/facebook/react
10. https://github.com/vercel/next.js
11. https://github.com/GoogleChrome/lighthouse
12. https://github.com/WebAssembly/binaryen

If you complete these 12 deeply, your fundamentals will be very strong.

---

## 1) Week 0.5 - CPU, Memory, OS Foundations

- https://github.com/nand2tetris/projects
- https://github.com/mit-pdos/xv6-riscv
- https://github.com/torvalds/linux
- https://github.com/0xAX/linux-insides
- https://github.com/riscv/riscv-isa-manual
- https://github.com/google/benchmark
- https://github.com/brendangregg/FlameGraph
- https://github.com/iovisor/bcc
- https://github.com/bpftrace/bpftrace

Study order:
1. `nand2tetris` -> `xv6`
2. `linux-insides` + `riscv-isa-manual`
3. `google/benchmark` + `FlameGraph`

---

## 2) Week 1 - Networking from Scratch

- https://github.com/beejjorgensen/bgnet
- https://github.com/the-tcpdump-group/tcpdump
- https://github.com/wireshark/wireshark
- https://github.com/curl/curl
- https://github.com/nginx/nginx
- https://github.com/libuv/libuv
- https://github.com/nodejs/llhttp
- https://github.com/nghttp2/nghttp2
- https://github.com/the-tcpdump-group/libpcap

Study order:
1. `bgnet` (concept + socket behavior)
2. `llhttp` (parser design)
3. `tcpdump/libpcap` (wire-level truth)

---

## 3) Weeks 2-3 - Browser Engine

- https://github.com/servo/servo
- https://github.com/LadybirdBrowser/ladybird
- https://github.com/LadybirdBrowser/ladybird/tree/master/Libraries/LibWeb
- https://github.com/WebKit/WebKit
- https://github.com/chromium/chromium
- https://github.com/whatwg/html
- https://github.com/w3c/csswg-drafts
- https://github.com/mozilla/gecko-dev

Study order:
1. `whatwg/html` + `csswg-drafts` (spec first)
2. `ladybird/LibWeb` (readable architecture)
3. `servo` (modern engine ideas)
4. `WebKit/Chromium` (production scale)

---

## 4) Weeks 4-5 - JavaScript Engine

- https://github.com/v8/v8
- https://github.com/v8/v8/tree/main/src
- https://github.com/bellard/quickjs
- https://github.com/boa-dev/boa
- https://github.com/boa-dev/boa/tree/main/core/engine
- https://github.com/facebook/hermes
- https://github.com/chakra-core/ChakraCore
- https://github.com/tc39/ecma262
- https://github.com/denoland/rusty_v8

Study order:
1. `quickjs` (small and understandable)
2. `boa` (Rust engine structure)
3. `v8` + `ecma262` (production + standard)

---

## 5) Week 6 - Virtual DOM and Reconciliation

- https://github.com/facebook/react
- https://github.com/facebook/react/tree/main/packages/react-reconciler
- https://github.com/preactjs/preact
- https://github.com/snabbdom/snabbdom
- https://github.com/infernojs/inferno
- https://github.com/acdlite/react-fiber-architecture
- https://github.com/krausest/js-framework-benchmark

Study order:
1. `acdlite/react-fiber-architecture`
2. `react-reconciler`
3. compare `preact` and `snabbdom`

---

## 6) Week 7 - React and Next.js Internals

- https://github.com/facebook/react
- https://github.com/reactjs/rfcs
- https://github.com/vercel/next.js
- https://github.com/vercel/next.js/tree/canary/packages/next/src/server
- https://github.com/vercel/next.js/tree/canary/packages/next

Study order:
1. React RFCs + reconciler
2. Next server internals
3. trace SSR/hydration flow in Next source

---

## 7) Week 8 - Performance Engineering

- https://github.com/GoogleChrome/lighthouse
- https://github.com/GoogleChrome/web-vitals
- https://github.com/ChromeDevTools/devtools-frontend
- https://github.com/webpack/webpack-bundle-analyzer
- https://github.com/evanw/esbuild
- https://github.com/rollup/rollup
- https://github.com/google/perfetto
- https://github.com/flamegraph-rs/flamegraph

Study order:
1. `web-vitals` + `lighthouse`
2. `devtools-frontend` + `perfetto`
3. bundler repos for build perf and code splitting

---

## 8) Week 9 - Engine Hacking + WASM

- https://github.com/v8/v8
- https://github.com/chromium/chromium
- https://github.com/WebKit/WebKit
- https://github.com/emscripten-core/emscripten
- https://github.com/WebAssembly/binaryen
- https://github.com/WebAssembly/wabt
- https://github.com/bytecodealliance/wasmtime

Study order:
1. `wabt` + `binaryen`
2. `emscripten`
3. runtime side (`wasmtime`) + browser engine integration

---

## 9) Week 10 - Embedded and Constraints

- https://github.com/zephyrproject-rtos/zephyr
- https://github.com/espressif/esp-idf
- https://github.com/lvgl/lvgl
- https://github.com/embedded-graphics/embedded-graphics
- https://github.com/mirror/busybox

Study order:
1. `zephyr` or `esp-idf` (platform)
2. `lvgl` / `embedded-graphics` (rendering constraints)
3. busybox-style minimal runtime mindset

---

## 10) How to Read Repos Efficiently

For each repo, do this in order:
1. `README`, `docs/`, architecture notes
2. build once and run tests/examples
3. map key directories and ownership
4. trace one end-to-end flow
5. patch one tiny behavior and rebuild

Use this note template:
- What problem this repo solves
- Core modules
- Hot paths
- Data structures worth copying
- One design tradeoff you agree with
- One design tradeoff you would change

---

## 11) If You Want Only 1 Path (No Overload)

- Months 1-2: `nand2tetris` + `xv6` + `bgnet`
- Months 3-4: `ladybird/LibWeb` + `quickjs`
- Month 5: `react-reconciler` + `next.js` server
- Month 6: `lighthouse` + `web-vitals` + `binaryen`

That path is enough to become very strong.
