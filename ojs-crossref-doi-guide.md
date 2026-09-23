# Setting Up Crossref DOIs in OJS

## About this guide

A Digital Object Identifier (DOI) is a persistent identifier for a scholarly work. Its registered URL can be updated when the location of the work changes, allowing the DOI to remain the stable identifier.

For a journal using Open Journal Systems (OJS), registering article DOIs with Crossref involves several separate pieces: obtaining Crossref membership, configuring OJS's DOI plugin, connecting OJS to Crossref through the Crossref XML export plugin, and providing metadata that can pass Crossref's validation.

This guide follows that process from membership through the first deposit. It also covers metadata that can be added beyond the minimum needed to register a DOI.

## Before you begin

The journal should have:

* OJS 3.3 or later, with journal manager or site administrator access.
* A registered ISSN or eISSN.
* A principal contact and a technical support contact, including their names and email addresses.
* A budget for Crossref membership and content registration fees, unless the journal is covered by another membership arrangement or a fee-reduction program.

The journal does not necessarily have to become a direct Crossref member. A sponsoring organization, such as a library consortium or scholarly publishing service, can register content on behalf of journals that use its membership. This can reduce administrative work, although the sponsor may charge for its services.

Crossref also operates the GEM program, which provides fee-free membership and DOI registration for qualifying organizations in certain lower-income countries. Check the current eligibility requirements before budgeting for membership.

Crossref membership may also be restricted in some countries because of international sanctions, so eligibility should be confirmed before applying.

## Crossref membership

If the journal joins Crossref directly, the general process is:

1. Complete the Crossref membership application for the appropriate organization type.
2. Review and accept the membership terms.
3. Pay the membership fee invoiced by Crossref.
4. Receive the journal's DOI prefix.

The prefix begins with `10.` followed by a four-digit code, for example `10.1234`. The prefix identifies the organization responsible for registering the DOI; the rest of the DOI is generated as a suffix for the individual object.

Membership and content registration are separate charges. Crossref's membership fees depend on the applicable fee tier, while DOI registration carries separate content registration fees. As of 2026, Crossref has a lower entry tier of US$200 for member organizations operating on publishing revenue or expenses of up to US$1,000 annually. Content registration fees are invoiced separately on a quarterly basis.

Because Crossref's fees and eligibility rules can change, current pricing should be checked directly with Crossref before an application is submitted.

## Configuring DOI generation in OJS

OJS includes a DOI plugin. Its purpose is to create DOI identifiers within the journal system; it does not by itself send those identifiers and their metadata to Crossref.

As a journal manager, open **Settings → Website → Plugins** and locate the DOI plugin under **Public Identifier Plugins**. Enable it and open its settings. Depending on the OJS interface, the settings control may be hidden behind the blue arrow beside the plugin name.

Enter the Crossref prefix assigned to the journal and select the types of content for which OJS should generate DOIs. Depending on the journal's configuration, these can include issues, articles, galleys, and supplementary files.

Selecting a content type does not necessarily mean that every item of that type will automatically receive a DOI. DOI assignment also depends on the suffix-generation method.

### Choosing the DOI suffix

The suffix is the portion of the identifier that follows the Crossref prefix. OJS provides placeholders that can be combined into a pattern:

| Placeholder | OJS value         |
| ----------- | ----------------- |
| `%j`        | Journal initials  |
| `%v`        | Volume            |
| `%i`        | Issue             |
| `%Y`        | Year              |
| `%a`        | Article ID        |
| `%g`        | Galley ID         |
| `%f`        | File ID           |
| `%p`        | Page number       |
| `%x`        | Custom identifier |

For example, OJS's default article pattern is:

```text
%j.v%vi%i.%a
```

A resulting DOI might look like:

```text
10.5206/ijoh.v8i3.12345
```

The journal can instead use a custom pattern or assign suffixes manually. With custom patterns, however, the journal is responsible for ensuring that the resulting suffixes are unique within its DOI prefix.

PKP's guidance favors relatively simple DOI suffixes. A suffix does not need to contain information that makes the citation itself readable; an opaque identifier can function just as well. Simpler patterns can also reduce the risk of introducing errors into identifiers that have already been registered.

**Caution:** changing DOI assignments after registration requires care. The OJS documentation describes *Reassign DOIs* as an advanced operation that regenerates DOI assignments according to the current pattern. Because changing registered identifiers can have consequences for existing records, a pattern change should be tested outside the production journal before it is applied.

### Assigning DOIs to existing articles

A journal that already has published content does not necessarily have to assign identifiers one article at a time.

From the DOI plugin settings, use *Assign DOIs* to batch-assign identifiers to articles that do not already have one. Existing DOI assignments are not affected by this operation according to the workflow described in the OJS documentation.

The batch operation is associated with the default DOI pattern. After assigning the identifiers, the next stage is to send their metadata to Crossref through the export/registration plugin.

## Connecting OJS to Crossref

The DOI plugin and the Crossref export plugin perform different jobs.

The DOI plugin creates the identifier in OJS. The **Crossref Export/Registration Plugin** prepares the metadata and submits it to Crossref.

Open **Tools → Import/Export → Crossref Export/Registration Plugin** and configure the deposit information. The exact labels and placement of controls can vary between OJS versions.

The plugin requires a depositor name and email address, as well as the credentials needed to submit metadata to Crossref. The depositor does not necessarily have to be the person who arranged the journal's Crossref membership; it can be the journal's technical contact or another person responsible for deposits.

The plugin can be configured for the production environment or for Crossref's test API. For an initial setup, using the test environment can help identify metadata and schema problems before records are permanently registered.

Once the configuration has been saved, the journal can use either automatic or manual deposits. With automatic registration enabled, OJS can submit DOI metadata as content is created or published. For the first deposit, however, a manual submission provides an opportunity to inspect the process.

## Journal and article metadata

Crossref obtains much of the information required for a deposit from the journal's OJS settings and from the article itself.

The journal should have its title, initials, abbreviation, ISSN or eISSN, principal contact, and technical support contact completed. Publication information also needs to be complete for the content being deposited. Article page numbers should be present where applicable.

The original workflow identifies missing ISSNs and page numbers as common causes of rejected deposits. These should therefore be checked before attempting registration.

Beyond the minimum information needed for a deposit, additional metadata can make a Crossref record more useful for discovery, citation tracking, and reporting.

### References

Crossref recommends including reference information in deposited metadata. OJS can collect and process references through its references functionality and related plugin.

The reference workflow can be enabled through **Settings → Workflow → Submission → Metadata**, where the journal can enable reference metadata and, if appropriate, ask authors to provide their references during submission. The corresponding plugin can then be enabled through **Settings → Website → Plugins**.

The reference functionality can use the Crossref API to identify possible DOIs for references supplied as plain text. Reference lists can also be displayed on article landing pages and included in deposited metadata.

There is an important distinction here: including references in individual Crossref deposits is not the same thing as Crossref's broader membership requirements concerning reference linking. These should not be treated as identical requirements.

### Funding information

Funding information can be recorded when an article is supported by a grant. OJS's Funding Metadata plugin can submit funding information to Crossref and use the Open Funder Registry to identify funders.

This allows funding information to be associated with the article's DOI metadata rather than relying exclusively on authors or institutions to report funded outputs separately.

### Institutional and researcher identifiers

Free-text affiliations can be ambiguous because the same institution may appear under different names in different systems.

The OJS ROR plugin allows authors to associate institutional affiliations with Research Organization Registry (ROR) identifiers. Author-level ORCID iDs can provide a corresponding persistent identifier for researchers.

Together, these identifiers give downstream systems machine-readable information for distinguishing both authors and their institutions.

### Similarity checking and citation information

Some OJS plugins extend the journal's workflow beyond DOI registration itself.

The **Similarity Check** plugin can send manuscripts to an iThenticate account for similarity checking. The service requires a subscription to Crossref's Similarity Check service.

The **Cited-by** plugin can display citation information on article pages using article DOIs and citation data from services such as Crossref and Scopus. Availability and behavior depend on the OJS version and plugin implementation.

These features should therefore be treated as optional integrations rather than prerequisites for creating a Crossref DOI.

### Licensing and full-text information

Rights and licensing information may not be included automatically through every standard OJS-to-Crossref workflow. Where licensing information is important to the journal, the deposited Crossref record should be checked rather than assuming that all information visible in OJS has been transferred.

The Crossref XML export and administrative tools can provide additional options for metadata that is not included through the standard automated workflow.

## Making the first deposit

Once the DOI configuration and metadata are ready, the journal can make its first deposit either automatically or manually.

For an initial deposit, the manual route makes it easier to inspect the records before submitting them.

Open **Tools → Import/Export → Crossref Export/Registration Plugin** and go to the **Articles** area. Select the articles to be deposited. The interface can also be filtered to a particular issue using its search controls.

Before submission, enable the XML validation option when appropriate. The plugin can validate the generated XML and report potential problems before the submission is sent to Crossref. This is more practical for smaller deposits than for very large batches.

Select **Deposit** to submit the records.

### Inspecting the XML before submission

The journal can also export the generated XML instead of immediately depositing it. Select the desired articles and use **Export XML** to download the Crossref XML generated by OJS.

The exported file can then be checked with Crossref's own validation tools before the journal submits it. This provides an additional opportunity to identify structural or metadata problems.

If a DOI has already been registered outside OJS—for example, through another Crossref deposit method—the OJS plugin can be told that the DOI has already been submitted. The **Mark Active** function can be used in that situation to indicate that the DOI is already registered and prevent OJS from treating it as an unsubmitted record.

## Confirming the registration

A Crossref deposit does not necessarily become available immediately.

After registration, the item's status should change to **Active**, although there can be a delay while the metadata is processed. Once registration has completed, the DOI should resolve to the article's landing page.

The status of individual articles can be checked in the **Articles** tab of the Crossref export/registration plugin. If an item remains unregistered after a reasonable period, review the error information provided by OJS before attempting another submission.

A successful first deposit should therefore be considered complete only when both the Crossref registration status and the DOI resolution have been checked.

## Maintaining DOI records

DOI registration is not a one-time task. Article metadata can change after publication, and the Crossref record should be updated when relevant information changes.

For example, a journal may need to redeposit an article when its title, author list, abstract, or page range is corrected. Crossref stores the metadata supplied in the most recent deposit, so an updated deposit is necessary when the registered information changes.

For isolated corrections, the manual export path can be more appropriate than running a larger automatic deposit.

Reference metadata should also be kept current when corrections or other post-publication changes affect the reference list.

DOI suffix patterns require particular caution. A pattern change affects how future identifiers are generated, while operations that reassign existing DOIs can affect identifiers that have already been assigned. For this reason, changes to the DOI configuration should be evaluated and tested outside production before being applied to a live journal.

## Sources and version notes

This guide is based on publicly available documentation concerning Crossref, OJS, PKP plugins, and related scholarly-publishing infrastructure. It is a documentation-based guide rather than a report of independent testing.

OJS interfaces and plugin behavior can vary by version. Crossref fees, eligibility requirements, plugin capabilities, and administrative procedures can also change. Before carrying out a production deposit, verify the current Crossref requirements and the documentation corresponding to the specific OJS version in use.

Where a procedure is presented as a recommendation rather than a Crossref requirement, it should be understood as a practical workflow choice rather than a mandatory registration condition.

**Documentation status**
Based on Crossref and PKP documentation reviewed in September 2026. OJS workflows and Crossref requirements may change; verify current documentation before applying the procedure to a production journal.
