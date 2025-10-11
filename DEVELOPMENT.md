# Development Notes

## Project History

These E2 chips were created as a learning project and have been continuously updated and expanded over time. While I've made significant efforts to refactor and optimize the code, some older sections may not reflect my current coding standards—particularly areas I may have overlooked during updates.

## Original Design

Originally, this system was built for personal use without the intention of public release or easy modification. As a result, nearly everything was hard-coded, making even simple changes (like updating the bank name or membership costs) extremely time-consuming.

## Code Restructuring

I've since reviewed every E2 chip and restructured the code for easier customization:

### Configuration Methods

- **Single-file data:** Editable variables are placed at the top of the E2 file when only used within that chip
- **Shared data:** Common values used across multiple E2s have been moved to a centralized config file

This restructuring should make it significantly easier to modify frequently-changed values without hunting through the entire codebase.

## Code Quality

While the system is fully functional, please be aware that:
- Some sections may contain older coding patterns from early development
- Optimization levels may vary between different chips
- The codebase reflects a learning journey and continuous improvement process

## Future Improvements

Contributions and suggestions for further optimization are welcome. If you find areas that could be improved, feel free to open an issue or submit a pull request.
