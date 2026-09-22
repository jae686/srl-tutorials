# Filesystem

On SRL, as of 09/2026, the only filesystem available is the filesystem present on the CD.

## CD Filesystem

On your SRL project, all files and directories placed under the `cd` directory are copied into the cd filesystem.

*File names are subject to the 8.3 format.*

### CD specific options

The following options are specific to the CD filesystem, and are set on the projects `makefile`

- `SRL_MAX_CD_BACKGROUND_JOBS` : Specifies the maximum number of files GFS can open at once
- `SRL_MAX_CD_FILES` : Specifies the maximum number of files on a CD
- `SRL_MAX_CD_RETRIES` : Set the number of times to retry on unsuccessful read

### On folder navigation

In some projects, you might want to have a folder for `textures`, another for `models` , etc.

In order to access the files in a folder , you must use the [`SRL::Cd::ChangeDir()`](https://srl.reye.me/classSRL_1_1Cd_a8ecc01fe5081cdc06ce3aed9139777d9.html#a8ecc01fe5081cdc06ce3aed9139777d9) function to go into a directory.

Passing `NULL` to this function brings you back to the root directory.
