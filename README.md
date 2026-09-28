# agui-go-sdk

A thin Go SDK for emitting [AG-UI](https://github.com/ag-ui-protocol/ag-ui) protocol events over SSE, built on top of the official community Go SDK and [Gin](https://github.com/gin-gonic/gin).

## Install

```bash
go get github.com/HandsomeNPC/agui-go-sdk
```

## Usage

```go
import "github.com/HandsomeNPC/agui-go-sdk"

func handler(c *gin.Context) {
    emitter := agui.NewEmitter(c, threadID, runID)
    defer emitter.RunFinishedSuccess()

    emitter.RunStarted()

    msgID := emitter.TextStart()
    emitter.TextContent(msgID, "Hello, AG-UI!")
    emitter.TextEnd(msgID)
}
```

## Overview

- `emitter.go` — `Emitter` wraps a Gin request and the AG-UI SSE writer, exposing typed helpers for every event in the protocol lifecycle: run lifecycle (`RunStarted` / `RunFinishedSuccess` / `RunError`), steps, text messages, reasoning, tool calls, state snapshots / deltas, messages snapshots, activity, and custom events. Transport vs. encoding errors are tracked separately via `Err()` / `EncErr()`.
- `diff.go` — `DiffState(before, after)` computes a minimal [RFC 6902](https://datatracker.ietf.org/doc/html/rfc6902) JSON Patch operation sequence between two JSON-serializable values, used by `Emitter.StateDeltaFrom`.
- `tdesign_helper.go` — `SuggestionTool` drives TDesign Chat's built-in `suggestion` tool.

## Status

Early / experimental.
