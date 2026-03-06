# ProseMirror History Fork Notes

## Why This Fork Exists

This package is vendored into the solution so the editor can override upstream `prosemirror-history` behavior without patching files under `node_modules`.

The fork is maintained as a standalone package because the upstream source should not be forced to satisfy the root workspace TypeScript rules. The root workspace uses stricter settings than the upstream package was written for, so this fork builds under its own local TypeScript configuration instead.

## How It Is Built

The root workspace script:

`npm run sync-prosemirror-forks`

does the following:

1. Builds this fork with `projects/prosemirror-history/tsconfig.json` into `projects/prosemirror-history/dist`.
2. Builds a CommonJS variant with `projects/prosemirror-history/tsconfig.cjs.json`.
3. Copies the CommonJS output to `projects/prosemirror-history/dist/index.cjs`.
4. Replaces `node_modules/prosemirror-history` with a junction to `projects/prosemirror-history`.

This ensures the application and any third-party runtime imports resolve to the local fork.

## What To Edit

Edit source files only:

- `projects/prosemirror-history/src/history.ts`
- `projects/prosemirror-history/src/index.ts`

Generated outputs under `projects/prosemirror-history/dist` are build artifacts and should not be edited manually.

## Build Commands

Rebuild just the forks:

`npm run sync-prosemirror-forks`

The root scripts already run this automatically before:

- `npm start`
- `npm run build`
- `npm run build-all`
- `npm run watch`
- `npm test`
- `npm install` / `npm ci` via `postinstall`

## Upstream Sync Rules

When pulling changes from upstream:

1. Update files under `src`.
2. Keep this package's local `tsconfig.json` and `tsconfig.cjs.json` intact unless the build model changes.
3. Re-run `npm run sync-prosemirror-forks`.
4. Verify the consuming editor still resolves `prosemirror-history` to this package.

## Local Behavior Changes

Document any intentional deviations from upstream here.

At the moment, this fork exists primarily to provide a maintainable local override point and isolated build path.
