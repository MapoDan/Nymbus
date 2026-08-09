# Nymbus — Folders and Tags API Contract

**Document type:** AFU — Organization metadata API  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Scope

Folders and tags organize notes without changing their cryptographic content.

## 2. Folders

The API supports:

- create;
- rename;
- move;
- list;
- delete;
- retrieve hierarchy.

## 3. Folder hierarchy

Folders may contain child folders. The backend must reject cycles and invalid parent references.

## 4. Folder deletion

The product must define whether deletion:

- moves child notes to an unfiled state;
- moves them to the parent folder;
- recursively deletes them.

The safe V1 default is to preserve notes and remove only the organizational association unless an explicit destructive action is selected.

## 5. Folder permissions

If folders are shared, the effective permission inheritance model must be applied server-side. A client must not infer permission from UI state.

## 6. Tags

The API supports:

- create tag;
- rename tag;
- delete tag;
- list tags;
- associate tag with note;
- remove association.

## 7. Tag uniqueness

Tag naming/uniqueness rules must be defined consistently within the user's scope.

## 8. Tags and privacy

Tags are intentionally server-readable metadata, including for private notes, because locked-note metadata search is a product requirement.

## 9. Authorization

A user may only modify tags/folders they own or are authorized to manage.

## 10. Synchronization

Folder/tag mutations must have stable IDs and synchronization metadata so offline clients can reconcile changes.
