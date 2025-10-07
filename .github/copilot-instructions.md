# GitHub Copilot Instructions for NSF Player

## Project Overview
This is a JavaScript library for playing NSF-format Nintendo Entertainment System music files in web browsers. It uses libgme (Game_Music_Emu) compiled with Emscripten for audio emulation.

## Code Style and Conventions

### JavaScript Standards
- Use ES6 modules (`import`/`export` syntax)
- Use `const` for constants and `let` for variables; avoid `var` except when maintaining existing legacy code
- Use arrow functions for callbacks and function expressions
- Use semicolons to terminate statements
- Use template literals for string interpolation (e.g., `` `Error: ${err}` ``)

### Browser Compatibility
- Support major modern browsers with Web Audio API
- Include fallbacks for legacy prefixed APIs (e.g., `webkitAudioContext`, `mozAudioContext`)
- Handle browser-specific methods like `createJavaScriptNode` (deprecated) and `createScriptProcessor`

### Audio Processing
- Always use Web Audio API for audio playback
- Buffer size should be `1024 * 16` for audio processing
- Audio nodes must be properly connected and disconnected to avoid memory leaks
- Save references to AudioContext and nodes to prevent garbage collection (e.g., `window.savedReferences`)

### Emscripten/libgme Integration
- Use `Module.ccall()` to call C functions from the compiled libgme library
- Always check return values from libgme functions for error handling
- Use `Module.allocate()` for memory allocation with appropriate type ('i32', 'i8*', etc.)
- Use `Module.getValue()` to read values from memory
- Use `Module.Pointer_stringify()` for converting C strings to JavaScript strings

### Error Handling
- Use `console.error()` for errors that should be logged
- Provide meaningful error messages
- Handle XMLHttpRequest errors gracefully (network, 404, etc.)

### Code Organization
- Export main API as async functions that return objects with methods
- Keep internal helper functions scoped within the main export
- Declare module-level variables before functions that use them

## Dependencies
- libgme.js - Emscripten-compiled Game_Music_Emu library (included in project)
- No external npm dependencies for runtime

## API Design
- Main export is `createNsfPlayer()` - an async function that returns a player object
- Player object provides `play(fileName, trackNo)` and `stop()` methods
- Optional audio context parameter for custom audio setups
- Track indexes start at 0

## Testing and Development
- No test framework is currently configured
- Manual testing with NSF files is required for validation
- Focus on browser compatibility when adding features
