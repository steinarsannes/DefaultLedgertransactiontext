# Default Ledgertransaction text
Extension for implenting project invoice customer text

The default text is set up in General ledger/Journal setup/Default descriptions

The texts marked "Project - invoice, customer" and "Projext -credit note,customer" is not implemented.

The code needed to add this functionality is


[ExtensionOf(classStr(CustVendVoucher))]
final class CustVendVoucher_ADVLegal_Extension
{
    protected final TransactionTxt initializeTransactionTxt(
        LanguageId _languageId,
        Voucher _referenceNumber,
        TransDate _date,
        LedgerTransTxt _transTxtType)
    {
        CustInvoiceJour custInvoiceJourLocal;

        TransactionTxt transactionTxt = next initializeTransactionTxt(_languageId, _referenceNumber, _date, _transTxtType);

        if (    TransactionTextContext::isTypeSupported(_transTxtType)
            &&  custVendInvoiceJour.TableId == tableNum(ProjInvoiceJour))
        {
            ProjInvoiceJour ProjInvoiceJourLocal = custVendInvoiceJour;
            ProjProposalJour ProjProposalJour;
            ProjInvoiceTable ProjInvoiceTable;

            TransactionTextContextProject ADVtransactionTextContext;
            switch (_transTxtType)
            {
                case LedgerTransTxt::ProjectCreditNoteCust :
                case LedgerTransTxt::ProjectInvoiceCust:
                    ADVtransactionTextContext = TransactionTextContext::newForTransactionType(_transTxtType);
                    ADVtransactionTextContext.setTableBuffer(ProjInvoiceJourLocal);
                    
					          ProjProposalJour = ProjProposalJour::find(ProjInvoiceJourLocal.ProposalId);
                    ADVtransactionTextContext.setTableBuffer(ProjProposalJour);

                    ProjInvoiceTable = ProjInvoiceTable::find(ProjInvoiceJourLocal.ProjInvoiceProjId);
                    ADVtransactionTextContext.setTableBuffer(ProjInvoiceTable);

                    transactionTxt.setTransactionTextContext(ADVtransactionTextContext);
            }
        }

        return transactionTxt;
	  }

}

