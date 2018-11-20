---

copyright:
years: 2018
lastupdated: "2018-11-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Using the Compare and Comply Tooling
{: #using_tool}

The service provides Compare and Comply Tooling to enable you to work with governing documents in a GUI environment. This short tutorial introduces the Tooling and takes you through the process of uploading and analyzing documents, then working with the results.
{: shortdesc}

The Compare and Comply Tooling currently accepts only PDF files as input. If you want to use another input format such as image or text, use the APIs instead of the Tooling.
{: important}

## Before you begin
{: #before-you-begin}

You need the following before you can use the Compare and Comply Tooling:

 - An IBM Cloud Private account.
 - A Compare and Comply service instance. If you already have a service instance, go to Step 1. If you do not have a service instance, see [Getting Started](/docs/services/compare-and-comply/getting-started.html).
 - Service credentials for the service instance as described in [Before you begin in Getting Started](/docs/services/compare-and-comply/getting-started.html#before-you-begin).
 
## Step 1: Launch the Compare and Comply Tooling
{: #step1}

1.  Log in to the {{site.data.keyword.icpfull_notm}} management console.
1.  From the main menu, expand **Workloads**, and then choose **Deployments**.
1.  Filter the list of deployments by the `default` or `cnc` namespace.
1.  Find the deployment named `{release-name}-ui`.

    If you don't know the `{release-name}`, search on `-ui` to find it.
1.  Click **Launch**.

    A new web browser tab opens and shows the {{site.data.keyword.conversationshort}} tool login page. For example: `https://compare-and-comply.{icp-url}`.
1.  Log in by using the same credentials you used to log into the {{site.data.keyword.icpfull_notm}} dashboard.

## Step 2: Upload a document
{: #step2}

The Tooling launches and displays the landing page. Click **Upload a document**. The Tooling opens a file browser. Select a PDF file and click **Open**.

The maximum file size for the Tooling is 1.5 MB.
{: important}

![Compare and Comply Tooling landing page](images/tool-landing.png)

## Step 3: Filter elements by labels
{: #step3}

The Compare and Comply Tooling displays three panes. The top pane lists the name of the PDF file and provides the **Upload New Contract** button. The left-side pane enables you to select specific labeled elements to display. The right-side pane displays the document in HTML format.

![Compare and Comply Tooling with opened document](images/tooling-open-doc.png)
 
1. In the left-hand pane, select one or more labels from the **Category** listings. When you select an item, the Tooling highlights the elements in that category. For example, selecting **Dispute Resolution** highlights all elements in the document that match that category.

  Category selections are logical `AND` operations. That is, if you select more than one category, the Tooling highlights only the elements that match all of the selections.
  {: note}
  ![Compare and Comply Tooling with category selection](images/tooling-category.png)
 
1. After selecting the category, you can select one or more active labels from the **Nature** listings, the **Party** listings, or both. As you make additional selections, the highlights change to match the combination of selected labels.

  Nature and party selections are logical `OR` operations. That is, if you select more than one nature or party, the Tooling highlights all elements that match any of the selections.
  {: note}
  ![Compare and Comply Tooling with category and nature selections](images/tooling-cat-nature.png)
 
1. Click a highlighted element to display all labels applied to the element. You can optionally provide _feedback_ to the labels and elements, as described in [Adding feedback](#tool-add-feedback).
   ![Compare and Comply Tooling displaying element-specific information](images/tool-highlight.png)

1. Use the up and down arrows to the right of the document to cycle through elements that match the specified labels.

1. Optionally, click **Clear All Labels** to unselect all labels.

1. Optionally, open another document by clicking **Upload New Contract**. The Tooling opens a file browser. Proceed as described in Step 2.
   ![Compare and Comply Tooling: Upload a new contract](images/tooling-replace.png)

See [Understanding contract parsing](/docs/services/compare-and-comply/parsing.html#contract_parsing) for listings and descriptions of all available categories, natures, and parties.

## Adding feedback
{: #tool-add-feedback}

As noted in Step 3, you can double-click any highlighted element in the Compare and Comply Tooling to display a pop-up showing the labels that are currently applied to the element. If you disagree with the labels, you can provide feedback to be reviewed and potentially incorporated into future updates to the service's learning model.

For more detailed information about feedback, see [Using the feedback APIs](/docs/services/compare-and-comply/feedback.html#feedback).

To provide feedback in the Tooling, perform the following steps:

1. As described in the previous steps in this topic, open the Tooling, open a document, and apply the labels you want to examine. Double-click on a highlighted passage to display the informational pop-up.

1. If you disagree with the information displayed in the pop-up, click the **Report Feedback** link. The **Feedback** panel opens.
   ![Compare and Comply Tooling: Feedback panel](images/tool-feedback-panel.png).
   
1. The panel lists each labeled element in the highlighted passage. Perform one or more of the following actions:
   - Mark the label as incorrect by clicking the ![Feedback incorrect icon](images/mark-icon.png) icon. The Tooling displays the status message **Marked incorrect** for the element. If you click the icon by accident or change your mind at a later time, click the ![Undo icon](images/tool-undo.png) icon.
   - Suggest a different label for the element by clicking **+ Suggest Label** and selecting a label from the drop-down menu. The Tooling displays the status message **Suggested**. You can undo the suggestion by clicking the  ![Undo icon](images/tool-undo.png) icon.
   - Explain about your feedback by clicking **Add Comments (optional)** and entering a brief text description.
   
   To dismiss the **Feedback** panel and abandon your feedback, click the **X** in the upper right-hand corner of the panel.
   
1. When you have finished providing feedback on the highlighted element, click **Report Feedback**.

The following screen image shows the **Feedback** panel with feedback provided for an arbitrary element.

![Compare and Comply Tooling: Completed Feedback panel](images/tool-feedback-filled.png)

### Reviewing feedback
{: #review_feedback}

Optionally, review some or all feedback in the document. Above the pane that shows the HTML version of your document, the Tooling displays a **Feedback Markers** toggle and the number of feedback instances that have been made to the document. By default, the toggle is enabled to show all elements in the document that have received feedback. Elements with feedback are highlighted and have a blue dot (![blue dot](images/blue-dot.png)) in the left-hand margin. You can perform any or none of the following actions:
   - Turn off feedback markers by clicking off the **Feedback Markers** toggle.
   - Double-click the highlight or the dot to open the informational pop-up. The pop-up includes a **Show reported feedback** drop-down; click it to review the previous feedback.
   - Provide different or additional feedback on the same element by clicking **Report Feedback** in the pop-up.