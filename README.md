# darling-libdispatch

## Upstream URL

https://github.com/apple-oss-distributions/libdispatch/tree/libdispatch-1271.120.2

## Updating Source

When updating the source code, make sure to refer to the [`Updating Sources`](https://docs.darlinghq.org/contributing/updating-sources/index.html#updating-sources) section from the Darling Docs website. Additional details (if any) can be found in [`darling/notes/UPDATE_SOURCE.md`](darling/notes/UPDATE_SOURCE.md).

## Workloops without native kevent workloop support

When `DISPATCH_USE_KEVENT_WORKLOOP` is disabled, workloops run on ordinary
root-queue workers. Their invocation temporarily installs the workloop's WLH
with the existing scoped fake/restore helpers, preserving the caller's exact
TLS value and ownership. This keeps nested serial/source drains' workloop QoS
and timer handling consistent with `DISPATCH_INVOKE_WORKLOOP_DRAIN`; the
anonymous root-worker sentinel is not a workloop object. The invocation's
existing queue ownership covers execution, without a new TLS storage reference.
Native kevent workloop delivery retains its existing adoption protocol.