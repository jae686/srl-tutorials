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
