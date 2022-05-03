---
# required metadata

title: Configure number sequences for warehouse flows
description: This topic provides an overview of the functionality that provides number sequence extensions for license plate IDs, wave label IDs, container IDs, and bill of lading IDs.
author: Mirzaab
ms.date: 06/10/2020
ms.topic: article
ms.prod:
ms.technology:

# optional metadata

ms.search.form: WHSNumberSequenceExt
# ROBOTS:
audience: Application User
# ms.devlang:
ms.reviewer: kamaybac
# ms.tgt_pltfrm:
# ms.custom:
ms.search.region: Global
# ms.search.industry: [leave blank for most, retail, public sector]
ms.author: mirzaab
ms.search.validFrom: 2020-06-10
ms.dyn365.ops.version: 10.0.2

---

# Configure number sequences for warehouse flows

[!include [banner](../includes/banner.md)]

The *Number sequence extensions* feature adds a new configuration page for setting up number sequences. It enables flexible configuration of GS1-regulated IDs, including GS1 prefixes and check digits (modulo 10), and enforces a length limit on existing number sequences.

Standard number sequence segments aren't suitable for GS1 implementation, because no check digit is calculated, and the company's GS1 prefix must be updated manually.

This feature adds the following functionality:

- Bill of lading (BOL) IDs can be generated in advance.
- A unique number sequence can be generated for serial shipping container code (SSCC) numbers.
- GS1-compliant number sequences can be created for BOL and SSCC numbers. The feature adds out-of-box support for license plate IDs, container IDs, wave label IDs, and BOL IDs.
- Configuration of license plate ID numbers is flexible. For example, you can include or exclude application identifiers (AI), such as leading zeros (00).

This functionality makes it more efficient to support labeling of cartons and to adjust new numbers that are generated by the system.

## Turn on the Number sequence extensions feature

Before you can use the feature, it must be turned on in your system. Admins can use the [Feature management](../../fin-ops-core/fin-ops/get-started/feature-management/feature-management-overview.md) workspace to check the status of the feature and turn it on if it's required. There, the feature is listed in the following way:

- **Module:** *Warehouse management*
- **Feature name:** *Number sequence extensions*

## Set up the feature

To set up number sequence extensions in your system, follow these steps.

1. Go to **Warehouse management \> Setup \> Warehouse management parameters**.
1. On the **General** tab, in the **GS1 company prefix** field, and enter your company's GS1 prefix. This value will affect all number sequences where the GS1 prefix is included as a segment.
1. If you want to generate BOL numbers for wave labels, on the **Reports** tab, select the **Generate BOL number when printing wave labels** check box.

    > [!NOTE]
    > This check box is available only if the functionality for [wave label printing](configure-wave-label-printing.md) is turned on.

1. Go to **Warehouse management** \> **Setup** \> **Number sequence extensions**
1. On the Action Pane, select **Create default setup**. A GS1-compliant BOL number sequence and three types of SSCC number sequences are created. All these number sequences take the length of your company's GS1 prefix into account.

    For more information about how to customize these default number sequences and/or add new sequences, see the next section. You can also remove any of these sequences if you don't need them.

1. Go back to **Warehouse management \> Setup \> Warehouse management parameters**.
1. On the **Number sequences** tab, select a relevant number sequence extension to use to generate numbers for your license plate IDs, wave label IDs, container IDs (in this case, select the appropriate **SSCC-18 number** sequence), and/or BOL IDs (in this case, select the **BOL** sequence). By default, number sequence extensions are supported only for these four types of IDs.

The next time that a new number is generated for one of these number sequences, the new logic will be used.

> [!NOTE]
> If you don't select a number sequence extension for license plate IDs, the current rules for generating license plate IDs will be used. Otherwise, your selected number sequence will be used. The other IDs will use a plain number sequence until you apply a number sequence extension for them.

## Create and edit number sequences

In the previous section, you generated a default set of number sequences. This section explains how each number sequence is defined. It also explains how to create custom sequences, and how to edit the default or custom sequences.

To create and edit number sequences, follow these steps.

1. Go to **Warehouse management** \> **Setup** \> **Number sequence extensions**.
1. On the Action Pane, select **New**.
1. In the **Number sequence extension** field, enter a name for the new sequence. In the **Description** field, enter a description.
1. On the **Segments** FastTab, use the buttons on the toolbar to assemble your numbering format by adding, deleting, and arranging segments. In the **Segment** field for each row, assign a segment type to define the purpose and content of that segment. The following table describes the types of segments that are available.

    | Segment type | Description |
    |---|---|
    | Constant | This segment type adds the same constant text for each generated number in the sequence. In the **Value** field, enter the required text. The **Length** field is automatically updated to the length of the text that you entered in the **Value** field. |
    | Number sequence | In the **Value** field, enter a number sign (*\#*) for each character that should be shown in the generated sequence. The number sequence itself might generate longer numbers, but only the right-most characters will be shown. The **Length** field is automatically updated to the number of number signs that you entered in the **Value** field.<p>To comply with GS1 requirements for SSCC-18 numbers, make sure that the length of this segment is 16 minus the length of your GS1 prefix.</p> |
    | GS1 prefix | This segment type adds the value that is set in the **GS1 company prefix** field on the **Warehouse management parameters** page. The **Value** field shows the value that is set on the **Warehouse management parameters** page, and the **Length** field shows the number of characters in the value. Both the **Value** field and the **Length** field are read-only. |
    | Application identifier | In the **Value** field, enter an application identifier, as specified by the relevant GS1 policy for this type of number sequence. For example, enter *00* for SSCC or *420* for BOL. The **Length** field is automatically updated to the length of the identifier that you entered in the **Value** field. |
    | Packing type | For items that can be clearly identified, this segment type adds a field value from the relevant unit sequence group (from the **Unit sequence groups** page). (This behavior matches the existing logic for license plate IDs.) For license plates that include multiple stock keeping units (SKUs), this segment type adds *0* (zero) by default. For this segment type, the **Value** field is always set to *P*, and the **Length** field is always set to *1*.|
    | Check digit | This segment type adds a check digit, which is a modulo 10 calculation. (This behavior matches the existing logic for license plate IDs.) For this segment type, the **Value** field is always set to a caret (*^*), and the **Length** field is always set to *1*. |

1. To view an example of your final number format, inspect the **Format** field at the bottom of the **Segments** FastTab.


[!INCLUDE[footer-include](../../includes/footer-banner.md)]