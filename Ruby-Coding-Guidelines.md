These style guidelines apply to all Mulberry Ruby code.

## Pathing

Certain Unix systems, especially Ubuntu on TravisCI, have difficulty handling relative paths. Always refer to directories based off of `Mulberry::Framework::Directories.root` (or make an appropriate helper method in `Directories`) which will ensure absolute paths get used.