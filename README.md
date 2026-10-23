# Agents Inbox

A [Paseo](https://paseo.sh) plugin: agents ask you a question or request an approval through an MCP tool, you answer in an Inbox screen, and the plugin wakes the agent with your answer. See [OVERVIEW.md](OVERVIEW.md).

## Development

```sh
npm ci
npm run typecheck && npm test && npm run build
```

CI runs the same through Dagger (`dagger call all`) in a `ci` GitHub Actions job.

## License

MIT
