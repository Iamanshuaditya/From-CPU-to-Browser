# Tiny HTTP Server Debugging Tips

## Symptom: Random 400s on valid requests
Check:
1. Incremental buffer boundary logic.
2. CRLF handling correctness.
3. Header continuation assumptions.

## Symptom: Requests hang
Check:
1. Blocking read without timeout.
2. Not consuming full request body.
3. Write path partial-send handling.

## Symptom: Keep-alive breaks on second request
Check:
1. Remaining bytes in buffer after first parse.
2. Content-Length accounting.
3. Parser reset state between requests.

## Symptom: Memory growth over time
Check:
1. Per-connection buffers never released.
2. Large temporary allocations in parser.
3. Logging queues without cap.

## Tool Workflow
- Use `tcpdump`/Wireshark for wire-level truth.
- Use app logs with request ids for correlation.
- Reproduce with a deterministic request corpus.

Related playbook:
- [../../debugging/network-debugging.md](../../debugging/network-debugging.md)
