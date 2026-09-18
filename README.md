# zermelo_doc

Public hosting for the **Zermelo Homework** iOS app's privacy policy.

`index.md` is a verbatim copy of `docs/privacy-policy.md` in the app repository
(private), with only a YAML front-matter block prepended so GitHub Pages renders it.

**The app repository is the source of truth.** Never edit `index.md` here to fix a
problem — that is exactly the drift the app repo's `Scripts/check-privacy-policy.sh`
rule (P6) exists to detect. Change the policy there, then re-copy and re-deploy.

To verify the published page still matches the source, from the app repository run:

    ZERMELO_PRIVACY_POLICY_URL='https://fercp.github.io/zermelo_doc/' bash Scripts/check-privacy-policy.sh
