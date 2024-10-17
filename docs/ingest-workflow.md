# Acquisition + ingest workflow for a-file order groups

## Prerequisites
- Write access to the `Migrants and the State` Shared Drive
- Familiarity with the [Project glossary](glossary.md)
- Order group has been processed & files have been received

## Steps

### Organize & catalogue the order group in Google Drive
1. Find the spreadsheet `ORDER GROUPS – INGEST TRACKING` within `DATA` in the shared Drive. On the first tab `ORDER GROUPS`, create a new row for the order group (e.g., `OG-YEAR-EXAMPLE`) and enter data for the first 3 columns.
2. Copy the second tab `OG-TEMPLATE` as a new tab named for the new order group (e.g., `OG-YEAR-EXAMPLE`). Then add each a-file as a row and fill out all the columns except `page_count`
3. Back in the `DATA` folder of the `Migrants and the State` Shared Drive, create a new folder named for the order group (e.g., `OG-YEAR-EXAMPLE`).
4. Within the new folder, create a README document for notes about the order and a subfolder called `Data`. Then upload all of the PDFs for the order into `Data`, ensuring that each PDF has the a-number in the file name.
5. Go back to the `ORDER GROUPS` tab in `ORDER GROUPS – INGEST TRACKING` and add the `GDRIVE FILE COUNT` and `GDRIVE LINK` to the `Data` folder of PDFs for the order group.

### Back files up to Research Workspace
6. Connect to the Research Workspace (RW) mount (See: [instructions](research-workspace.md))
7. Create a folder named after the order group (e.g., `OG-YEAR-EXAMPLE`) in the RW space and create subfolder in it named `pdfs`
8. Download the order group PDFs from Google Drive and put them in `pdfs`. Stay connected to the RW mount for the next steps.

### Preprocess files for IIIF
10. Go to [migrants-and-the-state/og-template](https://github.com/migrants-and-the-state/og-template) use click "Use this template" to create a new repository named for your order group (all lowercase) within the `migrants-and-the-state` GitHub org.
11. Clone the new repo to your local machine `cd` into it, and open it in your editor (e.g., VS Code)
12. Change the name & description in the README.
13. Update the `config.yml` file with the order name as the `label` and the `source_dir` as the path to a `jpgs` folder (which will get created later) within the order group on RW. (e.g., `/Volumes/migrants_state/OG-YEAR-EXAMPLE/jpgs`)
14. Create a `.env` file in the project root with AWS secrets/credentials (ask Marii)
15. Download the catalog sheet you made within `ORDER GROUPS – INGEST TRACKING` for the a-files as a CSV. Rename it to `records.csv` and put it within the `src` folder in the project root
16. Install/update ruby & gems:
    ``` sh
    asdf install ruby && bundle install
    ```  
17. Run task to create list of anums to txt file on RW:
    ``` sh
    bundle exec rake pdfs:anum_txt
    ```
19. Run task to add page count to CSV:
    ``` sh
    bundle exec rake pdfs:page_count_csv
    ```
21. Replace Google sheet in Drive with the CSV you just created to include page count info
22. Split the PDFs to JPGs on RW (creates the aforementioned `/Volumes/migrants_state/OG-YEAR-EXAMPLE/jpgs`):
    ``` sh
    bundle exec rake pdfs:split_jpgs
    ```

### Publish with IIIF APIs, create dashboard site
23. Use aperitiiif to build pyramidal tifs, IIIF json, and site HTML:
    ``` sh
    bundle exec aperitiiif batch build
    ```
24. Upload tifs to AWS S3
    ``` sh
    bundle exec rake s3:push:tifs 
    ```
25. Upload json to AWS S#
    ``` sh
    bundle exec rake s3:push:json
    ```
26. Git commit and push repo changes (including built HTML) to trigger deploy to github pages site
