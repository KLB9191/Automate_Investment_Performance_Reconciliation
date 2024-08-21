****Project: ****

The Fund_Composite_Reconciliation.py script will reconcile the QoQ fund composite changes in total originated capital, realized proceeds, fair market value, investment IRR, and cash yield. For each of these areas of recon, deal will be tagged for further review if certain conditions are met. Below are descriptions of what occurs in each area of the script:

**Lines 0 - 310**

The modules and classes are imported, current and previous end dates are specificied, and a connection to the Enterprisedatawarehouse is established. The current and previous fund composite is pulled from the Enterprise.dbo.ods_FundComposite view, the current and previous repayments are pulled from EnterpriseDatawarehouse.edw.InvestmentLedger, the Borrower Master table is pulled from EnterpriseDatawarehouse.edw.Borrower_Master, the previous and current quarter buys(Total Originated Capital) are pulled from EnterpriseDatawarehouse.edw.InvestmentLedger, the previous and current taxlots are pulled from the EnterpriseDatawarehouse.dbo.ods_TaxLotQuarterEnd, and the previous and current quarter accrued interest data are pulled from the EnterpriseDatawarehouse.dbo.ods_IRRQuarterEnd view. The change in valuation dataframe is also created by subtracting previous quarter taxlot fmv from current quarter taxlot fmv. 

**Lines 311 - 484**

Current quarter non-accrual data is pulled from EnterpriseDatawarehouse.dbo.ods_TaxLotQuarterend, realized proceeds are pulled in from EnterpriseDatawarehouse.dbo.ods_IRRQuarterEndCashflow, and realized proceeds topsides are pulled from EnterpriseDatawarehouse.edw.Fund_Composite_Topsides. The Change_in_AI dataframe is also created by subtracting the previous quarter accrued interest from the current quarter accrued intereset. Data cleaning is performed on these tables as well as some of the tables pulled in the previous section. These tables are then joined to the main recon dataframe in preparation for the realized proceeds reconciliation. Columns involved in the reconciliation of realized proceeds are also created, including a few that are populated with specific values based on certain conditions.

**Lines 485 - 712**

The buys, change_in_valuation, change_in_AI dataframes are joined to the main recon dataframe, some transformations are made, and additional columns involved in the reconciliation of the qoq fmv deltas and the qoq IRR deltas are created, including a few that are assigned specific values based on certain conditions. Next, the previous and current PIK and the previous and current cash rate data are pulled from the EnterpriseDatawarehouse.dbo.ods_TaxlotQuarterEnd. After data cleaning and transformations are performed on these dataframes, they are joined to the main recon dataframe in preparation for the qoq cash yield delta reconciliation.

**Lines 713 - 864**

Additional columns involved in the cash yield recon and the buys recon are created, including several that are assigned specific values based on certain conditions. Next, deals (rows) in the buys, realized proceeds, fmv, IRR, and cash yield recon that were marked for further review are consolidated and are assigned to a separate object, which later becomes the ‘Summary’ tab in the final Excel export. The purpose of this tab is to quickly allow the user to see which deals need further investigation. After final data cleaning is complete, the main recon dataframe, summary dataframe, as well as the supporting dataframes mentioned in the previous sections are assigned to separate tabs in the eventual final Excel export.
