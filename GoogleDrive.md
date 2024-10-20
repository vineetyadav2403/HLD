# Google Drive
## Requirements
### Functional Requirements
- Upload a file
- Download a file
- Automatically sync files across devices
### Non Functional Requirements
- availability >> consistency
- low latency upload and download
- support large file as 50GB
  - resume uploads
- high data integrity (sync accuracy)
### Out of scope
- roll own blob storage
## Core Entities
### File (raw bytes)
| Column name  | datatype | comment  |
|--------------|----------|----------|
### File Metadata
| Column name | datatype                             | comment |
|-------------|--------------------------------------|---------|
| file_id     | int                                  | pk      |
| folder id   | varchar                              |
| file_name   | varchar                              |
| size        | int                                  |
| file_type   | varchar                              |
| user_id     | int                                  |
| s3_link     | varchar                              |
| created_at  | timestamp                            |
| updated_at  | timestamp                            |
| status      | enum                                 | started |
| chunks      | [{ id, status, s3link, updated_at }] | 
### Users
| Column name | datatype | comment |
|-------------|----------|---------|
| user_id     | int      | pk      |

## API
``` APIs
POST /file -> pre-signed url
body: FileMetadata

POST pre-signed url
body: file chunk

PATCH /file -> 200 OK
body: partial<File Metadata> (chunks status updates)

GET /file/download/:fileId -> File & File Metadata

GET /changes?since={timestamp} -> File Metadata[]
```
## High Level Design
![Google Drive](/assets/google-drive.svg)