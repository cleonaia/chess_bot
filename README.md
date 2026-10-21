# Chess Bot

Chess Bot is a browser userscript that analyzes chess positions in real time and can suggest or execute moves using a UCI-compatible engine such as Stockfish. It is designed to work with Chess.com and Lichess through separate site adapters.

This project is intended for educational and personal analysis purposes. Using engine assistance or automation on Chess.com, Lichess, or any other chess platform may violate their Terms of Service and can lead to account restrictions or bans. Use it only where this is explicitly allowed.

## Features

- Real-time position analysis using Stockfish or another UCI-compatible engine
- Support for Chess.com and Lichess through modular site adapters
- Move suggestions with evaluation, principal variation, and mate detection
- Configurable search depth, thinking time, and move delay
- Optional premove support for faster responses
- Game history tracking and PGN import/export
- Post-game reports with accuracy and evaluation analysis
- In-page interface with board overlays and move scoresheets
- Persistent local settings
- Unit and integration tests using Vitest

## How it works

The application follows a modular pipeline:

```text
Chess website
   |
   v
Site adapter
   |
   v
Board state and move history
   |
   v
Engine service / UCI client
   |
   v
Best move and evaluation
   |
   v
User interface or move execution
```

1. The site adapter detects the active game, board orientation, legal moves, clocks, and opponent moves.
2. The state manager converts the current position into a format understood by the engine.
3. The engine service sends the position to Stockfish through the UCI protocol.
4. The engine returns the best move, evaluation, principal variation, and search information.
5. The interface displays the result through an overlay or panel.
6. Depending on the configuration, the bot either suggests a move or executes it automatically.

## Project structure

```text
chess-bot/
├── src/
│   ├── app/                 # Application controller, scheduler, and global state
│   ├── board/               # Board input, overlays, and ownership detection
│   ├── chess/               # Chess rules, notation, positions, and variants
│   ├── engine/              # Engine service, UCI client, search, premoves, timing
│   ├── game/                # Game history, PGN handling, reports, and confirmation
│   ├── settings/            # Settings model, schema, and storage
│   ├── shared/              # Shared utilities and chess helpers
│   ├── sites/               # Chess.com and Lichess adapters
│   ├── ui/                  # Interface components, panel, icons, and styles
│   ├── userscript/          # Userscript metadata and menu integration
│   └── main.ts              # Application entry point
├── tests/
│   ├── integration/         # Integration tests
│   └── unit/                # Unit tests
├── dist/                    # Compiled userscript output
├── docs/                    # Architecture and development documentation
├── package.json
├── vite.config.ts
├── vitest.config.ts
└── tsconfig.json
```

## Requirements

- Node.js 18 or newer
- Bun or npm
- A userscript manager such as Tampermonkey or Violentmonkey
- A UCI-compatible chess engine, preferably Stockfish

## Installation

Clone the repository:

```bash
git clone [https://github.com/cleonaia/chess_bot.git](https://github.com/cleonaia/chess_bot.git)
cd chess_bot
```

Install dependencies:

```bash
bun install
```

Or with npm:

```bash
npm install
```

## Development

Start the development build:

```bash
bun run dev
```

Or:

```bash
npm run dev
```

## Build

Generate the production userscript:

```bash
bun run build
```

Or:

```bash
npm run build
```

The compiled userscript is placed in the `dist/` directory.

## Tests

Run the test suite:

```bash
bun run test
```

Or:

```bash
npm run test
```

## Installing the userscript

1. Build the project:

   ```bash
   bun run build
   ```

2. Open Tampermonkey or Violentmonkey.
3. Create a new userscript.
4. Copy the contents of `dist/chesscom.user.js` or `dist/bundle.user.js`.
5. Paste it into the userscript editor and save it.
6. Open a supported chess website and check that the Chess Bot panel appears.

## Configuration

Settings are available from the in-page panel and are stored locally in the browser.

| Setting | Description |
|---|---|
| Engine path | Path to the Stockfish or other UCI engine binary |
| Search depth | Maximum search depth used by the engine |
| Thinking time | Maximum time allowed per move |
| Move delay | Delay before a move is executed |
| Auto-move | Enables or disables automatic move execution |
| Premove | Enables moves before the opponent responds |
| Site | Selects the active site adapter |
| UI overlay | Shows evaluations, best moves, and analysis on the board |

## Supported platforms

| Platform | Status |
|---|---|
| Chess.com | Supported |
| Lichess | Supported |
| Other sites | Planned |

## Engine integration

Chess Bot communicates with engines through the UCI protocol.

```text
> position startpos moves e2e4 e7e5
> go movetime 1000

< info depth 18 score cp 34 pv e2e4 e7e5 g1f3
< bestmove g1f3
```

The bot parses the engine output to extract the best move, evaluation score, principal variation, search depth, and mate announcements.

## Game reports

After a game finishes, Chess Bot can generate a report that includes:

- Move-by-move accuracy
- Average centipawn loss
- Best-move agreement
- Blunders, mistakes, and inaccuracies
- Evaluation trend graph
- PGN export

## Development commands

```bash
# Install dependencies
bun install

# Start development mode
bun run dev

# Build for production
bun run build

# Run tests
bun run test
```

If you use npm, replace `bun` with `npm run`.

## Code style

The project uses TypeScript, Biome, Vite, and Vitest.

Before submitting changes, run:

```bash
bun run lint
bun run test
```

## Contributing

1. Fork the repository.
2. Create a feature branch:

   ```bash
   git checkout -b feature/my-feature
   ```

3. Make your changes.
4. Add or update tests where relevant.
5. Run the tests:

   ```bash
   bun run test
   ```

6. Commit the changes:

   ```bash
   git commit -m "Add: short description of the change"
   ```

7. Push the branch:

   ```bash
   git push origin feature/my-feature
   ```

8. Open a pull request.

## Roadmap

- Improve adaptive move timing
- Expand opening-book support
- Improve tablebase integration
- Add more detailed post-game analytics
- Support additional chess websites
- Improve UI customization

## License

This project is licensed under the terms of the [LICENSE](./LICENSE) file.

## Acknowledgements

- [Stockfish](https://stockfishchess.org/)
- [Vite](https://vitejs.dev/)
- [Vitest](https://vitest.dev/)
- [TypeScript](https://www.typescriptlang.org/)
