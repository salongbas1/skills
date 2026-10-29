# RFService.asmx — Endpoint index (generated from RFService.cs)

จำนวน WebMethod ที่แยกได้: **498** (SP-based 350, inline SQL 148). สร้างอัตโนมัติด้วย regex จึงเป็น **จุดเริ่มค้นหา ไม่ใช่สเปก** — ก่อนใช้จริงให้เปิดเมธอดใน `App_Code/RFService.cs` (`grep -n "public .* <Name>("`).

สัญลักษณ์: `SP:` = เรียก stored procedure (ชื่อ SP ในฐานข้อมูล `similanwms_*`) · `SQL:` = inline SQL (kind: tables) · ⚠ = ต่อสตริง SQL จากพารามิเตอร์ (เสี่ยง SQL injection) · ชนิดคืนเป็น `DataTable` เว้นแต่ระบุ


## Auth / Setup / Version / Menu (15)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `check_adjust_permission` | oUsername, oUserGroup | SQL: select: user_group_access_control,parameter_tbl ⚠ |
| `Check_Login` | oUsername, oPassword | SP: check_login |
| `CheckVersionHH` | oVersion | SP: check_version_HH |
| `compareVersionIsUpdate` → `bool` | mj, mn, bu, rv | SQL: select:  |
| `get_license` | oUUID | SP: rf_get_license_by_id |
| `Get_Loading_Summary_Truck_License_Plate` | - | SP: get_loading_summary_truck_license_plate |
| `Get_Menu_HH` | oUsername | SP: get_menu_hh |
| `Get_Run_Number` → `string` | oDocType | SP: rf_run_number |
| `Get_User` | oClient | SQL: select: user_profile,customer_master ⚠ |
| `Get_Version` | - | SQL: select:  |
| `logApplicationlogHH` → `void` | oDT, oHH, oUser | SQL: insert: application_log_hh,HH ⚠ |
| `permission_using` | oUUID, oLicenseKey | SP: rf_permission_using |
| `RunDocRef` | oClient, oOrderType, oMaker | SP: rf_RunNumber, Login_Access |
| `selectPrinterList` | List<string[]> lsts | SQL: select:  |
| `UpdatePrinterList` → `bool` | List<string[]> lsts | SP: update_PrinterList |

## Receiving / Check-in / Return / Shelf-life (96)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `AddSerialNoPut` | client, item, receipt, serial, line, receipt_date, pallet, rec_num, user | SP: update_receipt_serial_putaway |
| `approve_shelflife` | oClient, oReceiptNo, oApprover | SQL: update: receipt_master ⚠ |
| `Branch_Checkin_Item_Order_Status_Detail` | oClient, oLoadingNo, oPackingNo | SP: web_branch_checkin_item_order_status_detail |
| `check_appr_shelflife` | oClient, oReceiptNo | SQL: select: receipt_master ⚠ |
| `check_approver_shelflife` | oApprover | SP: check_approver_shelflife |
| `check_create_receipt_return` | oClient, oReceiptNo, oCustomer, oRefNo | SP: check_create_receipt_return |
| `check_receipt_header_incoming` | oClient, oDate, oSupplier, oInc | SP: check_receipt_header_incoming |
| `check_shelflife` | oClient, oReceiptNo | SP: check_shelflife |
| `Check_Status_Receipt` | oClient, oOrderNo | SQL: select: receipt_master ⚠ |
| `Checkexpiry_shelf_life` | oClient, oItemNO, oPro_date, oExpi_date | SP: Checkexpiry_shelf_life |
| `Checkin_Receipt_Return_NEW` | oClient, oReceiptNo, oMaker | SP: receipt_checkin_batch |
| `Checkin_Receipt_Return_NEW_Check_differ` | oClient, oCustomer, oRefNo | SP: check_receipt_return_and_branch_return_differ_confirm |
| `CheckPendingForReceipt` | oClient, oReceipt | SP: check_pending_for_receipt |
| `CheckPendingForReceiptLine` | oClient, oReceipt, oLine | SP: check_pending_for_receipt_line |
| `Closed_Receipt` | oClient, oReceipt, oMaker | SP: rf_closed_receipt_order |
| `Closed_Receipt_Master` | oClient, oReceiptNo, oMaker | SP: closed_receipt_master |
| `Confirm_Branch_Receipt_Checkin_Detail` | oClient, oLoadingSummaryNo, oPackingNo, oMaker, oBranch | SP: web_confirm_branch_receipt_checkin_detail |
| `confirm_receipt_store_branch` | oClient, oReceiptNo, oMaker | SP: receipt_summary_store_barnch, check_pending_for_receipt, receipt_checkin_batch_branch |
| `Del_Branch_Receipt_Checkin_Detail` | oClient, oLoadingSummaryNo, oPackingNo, oQty, oMaker, oItemBarcode, oStatus, oBranch | SP: web_del_branch_receipt_checkin_detail |
| `Delete_Receipt_Check_Barcode` | oClient, oReceiptNo, oBarcode, oMaker | SP: delete_receipt_check_barcode |
| `DeleteReceiptLineSerial` | oClient, oReciptNo, oLineNo, oMaker | SP: delete_receipt_line |
| `DeleteReceiptSerial` | oClient, oReciptNo, oMaker | SP: delete_receipt |
| `DelLineReceiptDetail` | oClient, oReciptNo, oLineNo | SP: rf_delReceiptDetail |
| `ExportPDFDoc` → `string` | oType, oParam1, oParam2, oParam3, oParam4, oParam5, oMaker | SP: rf_reprint_pallet_checkin, next_pdf_number_clear |
| `Get_Branch_Receipt_Checkin_Branch_By_User` | oUser | SP: web_get_branch_receipt_checkin_branch_by_user |
| `Get_Branch_Receipt_Checkin_Detail_In_Packing` | oClient, oLoadingSummaryNo, oPackingNo, oBranch | SP: web_get_branch_receipt_checkin_detail_in_packing |
| `get_branch_return_for_receipt_return_header` | oClient, oCustomer, oDoc_no | SP: get_branch_return_for_receipt_return_header |
| `Get_Check_Barcode_From_ASN` | oClient, oASN, oBarcode | SP: get_check_barcode_from_asn |
| `Get_Check_Barcode_From_Po` | oClient, oPo, oBarcode
    //, oItemNo | SP: get_check_barcode_from_po |
| `Get_GR_For_Putaway` | oPallet | SP: rf_get_putaway_goods_receipt |
| `get_invoice_date_no` | oClient, oPo | SQL: select: purchase_orders ⚠ |
| `Get_Load_Summary_No_Check_Receipt_Type` | oClient, oLoadingSummaryNo, oBranch | SP: web_get_load_summary_no_receipt_type |
| `Get_LotByReceipt` | oClient, oReceipt | SQL: select: receipt_detail ⚠ |
| `Get_Pallet_List` | oClient, oReceiptNo | SP: get_pallet_for_receipt_waiting |
| `Get_Pallet_List_Detail` | oClient, oReceiptNo, oPalletNo | SP: get_pallet_for_receipt_waiting |
| `Get_PO` | oClient, oPONO, oReceiptType | SQL: select: purchase_orders ⚠ |
| `Get_Po_By_Barcode` | oClient, oReceipt_no, oBarcode | SP: get_po_by_barcode |
| `get_po_has_asn` | client, receipt_no | SQL: select: receipt_asn ⚠ |
| `Get_POtoGR` | oClient, oPONO, oWh, oBook | SQL: select: purchase_orders,order_lines,suppliers,receipt_detail ⚠ |
| `Get_Receipt_Detail` | oClient, oOrderNo | SQL: select: receipt_detail,product_master ⚠ |
| `Get_Receipt_List_Detail` | oClient, oReceiptNo | SP: get_pallet_for_receipt_waiting |
| `Get_Receipt_List_Detail_Closed` | oClient, oReceiptNo | SP: get_pallet_for_receipt_waiting |
| `Get_Receipt_Return_Differ_From_Branch_Return` | oClient, oCustomer, oRefNo | SP: check_receipt_return_and_branch_return_differ |
| `Get_Station_CheckIN` | oWarehouse | SQL: select: tms_station_master ⚠ |
| `get_summary_receipt_order` | oClient, oReceipt_no, oLoansummary | SP: summary_receipt_order |
| `GetASNReturn` | oClient, oASN, oWarehouse | SP: get_asn_return |
| `GetBranchReturn_Header` | oClient, oCustomer, oDocRef | SP: get_branch_return_for_receipt_return_header |
| `GetIncoming_Header` | oClient, oSupplier, oReceiptDate | SP: get_incomlist_for_receipt_header |
| `GetIncPO_Detail` | oClient, oSupplier, oInc_no, oPO, oItem, oReceipt_date | SP: get_po_for_receipt_shu |
| `GetIncPO_Detail_store_branch` | oClient, oSupplier, oInc_no, oPO, oItem, oReq_date | SP: get_incomlist_for_receipt_store_branch |
| `GetIncPO_Header` | oClient, oSupplier, oBook, oPO, oItem, oReceipt_date | SP: get_po_for_receipt_shu_header |
| `importItemDecription` | oItemNo | SQL: select: product_master ⚠ |
| `ImportSaleDetail` | oClient, oOrder, oOrderType | SQL: select: sales_details,product_master,sales_orders,warehouses ⚠ |
| `ImportSaleOrder` | oClient, oBook, oBranch, oOrder | SQL: select: sales_details,sales_orders,warehouses ⚠ |
| `importStockHistory` | oRecnum | SQL: select: pallet_history ⚠ |
| `importStockLocation` | oClient, oWarehouse, oItemNo, oLotNo, oGrade, oOI_NO | SQL: select: FROM,pallet_history ⚠ |
| `importStockLocHis` | oClient, oWarehouse, oZone, oItem, oLotNo, oGrade, oOI_NO | SQL: select: pallet_location,pallet_history,product_master,locations ⚠ |
| `importUser` | - | SQL: select: user_profile |
| `ins_barcode_wrong` | oClient, oBarcode_shelf, oBarcode, oMaker, oBranch | SP: ins_check_barcode_shelf_product |
| `Ins_Branch_Receipt_Checkin_Detail` | oClient, oLoadingSummaryNo, oPackingNo, oMaker, oItemBarcode, oStatus, oBranch, oReceiptType, oQty | SP: web_ins_branch_receipt_checkin_detail |
| `Insert_invoice_detail` → `bool` | oClient, oOrderNo | SP: insert_invoice_detail |
| `insertImportHistory` → `void` | oUser, oOrder_no | SQL: insert: import_history ⚠ |
| `InsupdReciptD` → `bool` | oClient, oReciptNo, oLineNo, oPONo, oPOLine, oPalletNo, oItemNo, oUOM, oGrade, oQTY | SP: insupd_Receipt_Detai |
| `InsupdReciptM` → `bool` | oClient, oReciptNo, oDocRef, oWarehouse, oStation, oSupplier, oCarrier, oVehicle, oContainer, oRemark, oStatus, oSO | SP: insupd_Receipt_Master |
| `Packing_Confirm_Pod` | oClient, oSalesOrder, oLoadno, oPackingNo, oRemark | SP: confirm_packing |
| `receipt_checkin` | oClient, oReciptNo, oUser | SP: receipt_checkin, assign_whse_task_manual |
| `receipt_checkin_by_pallet` | oClient, oReciptNo, oUser, oPallet | SP: receipt_checkin_by_pallet, assign_manual_putaway_task_pallet |
| `Receipt_Checkin_Closing` | oClient, oReceiptNo, oMaker, oReturnMsg | SP: rf_receipt_checkin_without_closing |
| `Receipt_Checkin_Closing_Line` | oClient, oReceiptNo, oLine, oMaker, oReturnMsg | SP: rf_receipt_checkin_without_closing_line |
| `Receipt_Get_Check_Barcode` | oClient, oReceiptNo, oBarcode | SP: get_receipt_check_barcode |
| `Receipt_Summary_Line` | oClient, oReceiptNo, oPallet, oMaker | SP: receipt_summary_line |
| `Receipt_Summary_Line_Store_Branch` | oClient, oReceiptNo, oPallet, oMaker | SP: receipt_summary_line_store_barnch |
| `reset_receipt_checkin_order` | oClient, oReceipt_no, oMaker, oPallet | SP: reset_receipt_checkin_order |
| `Reverse_Receipt_Pallet` | oClient, oReceiptNo, oPallet, oMaker | SP: reverse_trx_receipt_pallet |
| `Reverse_Trx_Receipt` | oClient, oReceipt, oMaker | SP: reverse_trx_receipt |
| `rfSelReceiptNoList` | oClient, oReciptNo, oReceiptType, oBook | SP: rf_sel_receipt_no_List |
| `rfSelReceiptNoListStoreBranch` | oClient, oReciptNo, oReceiptType, oBook, oStore_branch, oWarehouse | SP: rf_sel_receipt_no_List_store_branch |
| `rfSelReceiptPutAway` | oClient, oReciptNo, oUser | SP: rf_sel_receipt_Putaway |
| `rfSelReceiptReturnNoList` | oClient, oReciptNo, oReceiptType, oBook | SP: rf_sel_receipt_return_no_List |
| `setItemCodeRefASN` | client, order_no, asn_ref, item_no | SQL: select: product_master,product_uom,receipt_asn,receipt_master ⚠ |
| `setItemCodeRefPO` | client, item_no, po_ref | SQL: select: product_master,product_barcode,order_lines,purchase_receipt_detail_temp ⚠ |
| `Summary_Branch_Receipt_Checkin_Detail` | oClient, oLoadingSummaryNo, oPackingNo, oMaker, oBranch | SP: web_summary_branch_receipt_checkin_detail |
| `Undo_Receipt` | oClient, oReceiptNo, oMaker | SP: undo_receipt_checkin |
| `Undo_Receipt_by_line` | oClient, oReceiptNo, line, oMaker | SP: undo_receipt_checkin_byline |
| `Update_Receipt_Check_Barcode` | oClient, oReceiptNo, oBarcode, oMaker | SP: update_receipt_check_barcode |
| `Update_Receipt_Detail` | oClient, oReceiptNo, oPONO, oLine, oPallet, oItem, oUOM, oQTY, oGrade, oLocation, oLot, oBatch, oSupplier, oExpiry, oMfg, oSize, oColor, oClass, oMaker, oPoLine, oAsnLine | SP: rf_update_receipt_detail_udf |
| `Update_Receipt_Detail_NEW` | oClient, oReceiptNo, oReceiptDate, oIncoming, oPONO, oLine, oPallet, oItem, oBarcode, oUOM, oQTY, oGrade, oLot, oBatch, oExpiry, oMfg, oSize, oColor, oClass, oMaker, oDetailInvoice, oDetailInvoiceDate, oDetailInvoiceLine, asn_check, oQtyPerPallet, oWeightDetail | SP: update_receipt_detail_udf |
| `Update_Receipt_Detail_NEW_store_branch` | oClient, oReceiptNo, oReceiptDate, oIncoming, oPONO, oLine, oPallet, oItem, oBarcode, oUOM, oQTY, oGrade, oLot, oBatch, oExpiry, oMfg, oSize, oColor, oClass, oMaker, oDetailInvoice, oDetailInvoiceDate, oDetailInvoiceLine, oLocation | SP: update_receipt_detail_udf_store_branch |
| `Update_Receipt_Detail_Scan` | oClient, oReceiptNo, oPONO, oLine, oPallet, oItem, oUOM, oQTY, oGrade, oLocation, oLot, oBatch, oSupplier, oExpiry, oMfg, oSize, oColor, oClass, oMaker, oPoLine, oAsnLine | SP: rf_update_receipt_detail_udf_scan |
| `Update_Receipt_Detail_Scan_Serial` | oClient, oItem, oSerial, oReceipt, oLine, oReceiptDate, oPallet, oMaker | SP: update_receipt_serial |
| `update_receipt_detail_udf` | oClient, oReciptNo, oReceiptDate, oIncoming, oIncomingLine, oLine, oPallet, oItem, oUOM, oQTY, oGrade, oLocation, oBatch, oLot, oMaker, oBarcode, oRemark, oExpiry, oQtyPerPallet, oWeight | SP: update_receipt_detail_udf |
| `Update_Receipt_Header` | oClient, oReceiptNo, oStation, oWarehouse, oZone, oType, oCarrier, oVehicle, oContainer, oDate, oCustomer, oSupplier, oClientPoNo, oClientSoNo, oRemarks, oRefNo, oStatus, oMaker, oReply | SP: rf_update_receipt_header |
| `Update_Receipt_Header_NEW` | oClient, oBook, oReceiptNo, oReceiptDate, oDemandDate, oIncoming, oStation, oWarehouse, oZone, oType, oContainer, oSupplier, oClientSoNo, oRemarks, oRefNo, oStatus, oMaker, oInvoice, oInvoice_Date, oBranch, oClientPoNo, landed_cost, vatisout, oNetDiscount, oNetPrice, oNetValue, oVatAmount, oNetAmount, listBranch, oTruckplate, // <-- เพิ่ม
        string oDriver, // <-- เพิ่ม
        string oDistance, // <-- เพิ่ม
        string oWeight, // <-- เพิ่ม
        DateTime ShipDate       // <-- เพิ่ม | SP: update_receipt_master_bu |
| `Update_Receipt_Header_NEW_store_branch` | oClient, oBook, oReceiptNo, oReceiptDate, oIncoming, oStation, oWarehouse, oZone, oType, oContainer, oSupplier, oClientSoNo, oRemarks, oRefNo, oStatus, oMaker, oInvoice, oInvoice_Date, oStore_branch | SP: update_receipt_master_bu_store_branch |
| `Update_Receipt_Return_Header_NEW` | oClient, oBook, oReceiptNo, oReceiptDate, oIncoming, oStation, oWarehouse, oZone, oType, oContainer, oCustomer, oClientSoNo, oRemarks, oRefNo, oStatus, oMaker, oCheck_type, branch | SP: update_receipt_master_bu |
| `web_reset_branch_receipt_checkin_detail` | oClient, oLoadingSummaryNo, oPackingNo, oMaker, oBranch | SP: web_reset_branch_receipt_checkin_detail |

## Putaway (9)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `DeleteSerialNoPut` | client, item, receipt, serial, user | SP: delete_serial_no_putaway |
| `GenSerialPut` | client, item, receipt, line, receipt_date, pallet, qty, rec_num, user | SP: gen_serial_no_putaway |
| `Get_Assigned_ByPutaway` | oWO | SQL: select: works_orders ⚠ |
| `Get_Item_No_For_Putaway` | oGR, oItemNo | SP: rf_get_putaway_gr_item_no |
| `Get_Location_For_Putaway` | oClient, oWarehouse, oItemNo | SP: get_locationby_putaway |
| `Get_Pallet_For_Putaway` | oPallet | SP: rf_get_putaway_pallet |
| `Get_PutAway_Detail` | oOrderNo, oItem, oUser | SP: rf_get_putaway_gr_item_no2 |
| `GetDetailPalletForPutaway` | oPallet | SQL: select: stock_movements,product_master ⚠ |
| `Putaway` | oPallet, oWarehouse, oLocation, oOperation | SP: rf_putaway_stock_movements_pallet |

## Pick / Work order / Sales order (84)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `Assign_Manual_Pick_by_Order` | oClient, oMaker | SP: assign_manual_pick_by_order |
| `Assign_Manual_Pick_by_Order_new` | oClient, oMaker, oWo | SP: assign_manual_pick_by_order_new |
| `check_create_sales_return` | oClient, oBranch | SP: check_create_sales_return |
| `check_pallet_check_in_order` | oClient, oReceipt_no, oCustomer, oLoadsummary | SP: check_pallet_check_in_order |
| `CheckPendingForSales` | oClient, oSales | SP: check_pending_for_sales |
| `ClosePickTask` | oWO, oTaskNo, oActNO, oQtyPick, oUOM, oReasonCode, oMaker | SP: rf_close_pick_task |
| `ClosePickTaskEkapab` | oWO, oTaskNo, oActNO, oQtyPick, oReasonCode, oMaker, oUOM, oPalletTo | SP: rf_close_pick_task |
| `ClosePickTaskTranfer` | oWO, oTaskNo, oActNO, oQtyPick, oUOM, oReasonCode, oLocation, oMaker | SP: rf_close_pick_task |
| `Confirm_Works_Order_by_Order` | oClient, oSalesOrder, oWorkOrder, oMaker | SP: confirm_works_order_by_order |
| `Confirm_Works_Order_by_Pallet` | oClient, oSalesOrder, oWorkOrder, oMaker, oPalletToConfirm | SP: confirm_works_order_by_pallet |
| `Confirm_Works_Order_by_Task_New` | oClient, oSalesOrder, oWorkOrder, oMaker | SP: confirm_works_order_by_task_new |
| `create_GoodsIssue_Header` | client, order, order_date, order_type, maker, whse_source, zone, qcbook, qcbranch, customer | SP: upd_sales_order |
| `CreateStoreBranchReturnHeader` | oClient, oBranch, oMaker | SP: create_sales_return_header |
| `Delete_Loading_Summary_Detail` | oLoadingSummaryNo, oClient, oSalesOrder, oCustomer, oPackingNo, oMaker | SP: delete_loading_detail_in_packing_order |
| `Delete_Packing_Order` | oClient, oLoad, oPack | SQL: update: packing_order ⚠ |
| `DeleteLogLoadOrder` → `void` | oOrder, oUser | SQL: delete:  ⚠ |
| `DeleteReservationOrderDetail` | oClient, oBranch, oLine_no, oItem_no, oUom, oQty, oRemark, oSource, oMarker | SP: web_delete_branch_order_detail |
| `DeleteSerialNoSO` | oClient, oSO, oItem, oSerial, oMaker | SP: delete_serial_no |
| `Generate_Packing` | List<string[]> parameters | SP: insupd_packing_order |
| `Get_Detail_Tranfer_WorkOrder` | oWO | SP: rpt_tf_tasks |
| `Get_Detail_Tranfer_WorkOrderBySelect` | oWO, oTask, oAct | SP: rpt_tf_tasks |
| `Get_Detail_WorkOrder` | oWO | SP: rpt_os_tasks |
| `Get_Detail_WorkOrderBySelect` | oWO, oTask, oAct, oUser | SP: rpt_os_tasks |
| `Get_Item_Description_Branch_Order` | oClient, oItemNo | SQL: select: product_master,product_uom ⚠ |
| `Get_Loading_Sales_Order_Customer_Info` | oClient | SP: get_loading_sales_order_customer_info |
| `Get_Loading_Sales_Order_Customer_Info_by_Sales_Order` | oClient, oSalesOrder | SP: get_loading_sales_order_customer_info_by_sales_order |
| `Get_Order_Tranfer_Detail` | oClient, oOrder | SQL: select: sales_details ⚠ |
| `get_OrderList_GoodsIssue` | client, book, branch, start_date, end_date | SP: sel_orderlist_goods_issue_hh |
| `Get_OrderPickSum` | oClient, maker, wo | SP: get_wo_pick_by_order_new |
| `get_packing_order_new` | oClient, oSalesOrder, oPack_No | SP: get_packing_order_new |
| `Get_Packing_Orders_detail` | oClient, oPack_no | SQL: select: packing_order_detail,packing_order ⚠ |
| `Get_Sale_Order` | oOrder | SP: rf_show_sale_order |
| `Get_Sel_Orderlist_SI` | oClient, oBranch, oStart_date, oEnd_date | SP: sel_orderlist_SI_book10 |
| `Get_Sel_Orderlist_SI_HH` | oClient, oBranch, oStart_date, oEnd_date, oMaker | SP: sel_orderlist_SI_book10_hh |
| `Get_SO` | oClient, oOrderNo | SQL: select: sales_orders ⚠ |
| `Get_Stock_Pick_by_Order` | oClient, oItemNo, oUom, oQtyPick, oWarehouse, oZone, oGrade, oPallet | SP: get_stock_pick_by_order |
| `Get_Stock_Pick_by_Order_Carton` | oClient, oItemNo, oUom, oQtyPick, oWarehouse, oZone, oGrade, oPallet | SP: get_stock_pick_by_order_carton |
| `Get_Stock_Pick_by_Order_Carton_Replenishment` | oClient, oItemNo, oUom, oQtyPick, oWarehouse, oZone, oGrade, oPallet | SP: get_stock_pick_by_order_carton_replenishment |
| `Get_Stock_Pick_by_Order_New` | oClient, oItemNo, oUom, oQtyPick, oWarehouse, oZone, oGrade, oPallet, oWO | SP: get_stock_pick_by_order_new |
| `Get_Stock_Pick_by_Order_New_Replenishment` | oClient, oItemNo, oUom, oQtyPick, oWarehouse, oZone, oGrade, oPallet, oWO | SP: get_stock_pick_by_order_new_replenishment |
| `Get_Stock_Pick_by_Task_New` | oClient, oItemNo, oUom, oQtyPick, oWarehouse, oZone, oGrade, oPallet, oWO | SP: get_stock_pick_by_task_new |
| `get_transfer_order` | oClient, oDocRef, oBook | SP: get_transfer_order |
| `Get_WO_Pick_by_Order` | oClient, oMaker | SP: get_wo_pick_by_order |
| `Get_WO_Pick_by_Order_Detail` | oClient, oOrderNo, oWorksOrder | SP: get_wo_pick_by_order_detail |
| `Get_WO_Pick_by_Task_New` | oClient, oMaker, oWo | SP: get_wo_pick_by_task_new |
| `Get_WO_Pick_by_Task_New_Detail` | oClient, oOrderNo, oWorksOrder, oBarcode | SP: get_wo_pick_by_task_new_detail |
| `Get_WO_Pick_List_by_Task_New_Detail` | oClient, oOrderNo, oWorksOrder | SP: get_wo_pick_list_by_task_new_detail |
| `GetPickByOrderForReplenishment` | oClient, oWorksOrder, oOrderNo | SP: get_pick_by_order_for_replenishment |
| `GetPickSummary` | oClient, oPs, oItem | SQL: select: stock_movements,sales_orders,product_master,product_uom ⚠ |
| `GetReservationOrderDetail` | oClient, oBranch | SP: web_get_branch_order_detail |
| `GetSaleStoreReturnDetail` | oClient, oBranch, oDoc_no | SP: web_get_store_branch_return_detail |
| `GetTranferStockMovements` | oClient, oWO, oMaker | SP: rf_get_wo_stock_movements_Transfer |
| `GetTranferStockMovementsshop` | oClient, oWO, oMaker | SP: rf_get_wo_stock_movements_Transfer_shop |
| `GetWorkorderForPicktask` | - | SP: rf_get_workorder_for_picktask |
| `GetWoStockMovements` | oClient, oWO, oMaker | SP: rf_get_wo_stock_movements |
| `insupdTRANSFER_ORDER` | oClient, oOrderNo, oStatus, oMaker | SP: D_insupd_D_TRANSFER_ORDER |
| `Log_Sale_Order_Master` | oClient, oOrderNo, oCustomer, oStatus, oName, oAddress1, oAddress2, oCountry, oPostal, oRemark, oDate, oLoading, oMaker | SP: rf_log_sO_master |
| `packing_order_Abandon` | oClient, oLoad_no, oPack_no, oPicture_no | SP: upd_packing_order_Abandon |
| `Pick_By_Order_Get_Confirm_Uom_By_Barcode` | oClient, oItemNo, oBarcodeNo, oUom | SP: pick_by_order_get_confirm_uom_by_barcode |
| `Pick_Replenishment` | oClient, oSoNo, oReference, oWoNo, oLineNo, oQtyPick, oUom, oPallet_Fr, oWarehouse_Fr, oLocation_Fr, oItemNo, oBarcodeNo, oMaker | SP: rf_pick_replenishment |
| `Pick_Sales_Detail` | oClient, oSoNo, oReference, oWoNo, oLineNo, oQtyPick, oUom, oPallet_Fr, oWarehouse_Fr, oLocation_Fr, oItemNo, oBarcodeNo, oPallet_To, oMaker, oLocationAssign, PickConfirm | SP: rf_pick_sales_detail2 |
| `Pick_Sales_Detail_Task_New` | oClient, oSoNo, oReference, oWoNo, oLineNo, oQtyPick, oUom, oWarehouse_Fr, oItemNo, oBarcodeNo, oMaker, PickConfirm | SP: rf_pick_sales_detail_task_new |
| `PickingOrder` | oClient, oPs, oItem, oQtyNew, oLocation, oPalletFrom, oUsername | SP: rf_close_pick_summary |
| `ProductSerialGetSO` | oClient, oSO, oItem, oPallet, oLine | SP: product_serial_get |
| `ProductSerialGetSO_PickTask` | oClient, oItem, oSO, oPallet, oWO, oTask, oActivity | SP: product_serial_get |
| `Reverse_Trx_SIbyOrder_by_Line` | oClient, oSalesOrder, oWorkOrder, oTask, oActivity, oMaker | SP: reverse_trx_sibytask_new_by_line |
| `Reverse_Trx_SIbyOrder_by_Task` | oClient, oSalesOrder, oWorkOrder, oTask, oActivity, oMaker | SP: reverse_trx_sibyorder_by_task |
| `Save_Loading_Summary_Detail` | oLoadingSummaryNo, oClient, oSalesOrder, oCustomer, oPackingNo, oMaker | SP: update_loading_detail_in_packing_order |
| `SelTransferOrder` | oClient, oOrderNo | SP: D_sel_TransferOrder |
| `sequenCheckStatusOrder` → `bool` | oClient, oOrder | SQL: select: sales_orders ⚠ |
| `SerialFlagCheckSo` | client, sales_order, task, activity | SP: serial_flag_check_so |
| `SerialPendingGetSo` | oClient, oSO, oItem, oPallet, oLine, oReqQty | SP: serial_pending_get_so |
| `SerialPendingGetSO_PickTask` | oClient, oItem, oSO, oPallet, oQty, oWO, oTask, oActivity | SP: serial_pending_get |
| `Show_Pick_Replenishment` | oWorkOrder | SP: rpt_os_tasks_replenishment |
| `Show_Pick_Sales_Detail` | oWorkOrder | SP: rpt_os_tasks_si_by_order |
| `Show_Sale_Order` | oOrder | SP: rf_show_sale_order |
| `ShowSaleOrder` | oClient, oBook, oHH_No | SQL: select: sales_orders,customer_master,import_history ⚠ |
| `Update_Pick_Movement` | oWorks_order, oSales_order, oTask_no, oActivity, oPallet_to, oLocation_to, oMaker | SP: sp_update_pick_movement |
| `UpdateReservationOrderDetail` | oClient, oBranch, oLine_no, oItem_no, oUom, oQty, oRemark, oSource, oMarker, flag | SP: web_update_branch_order_detail |
| `updateSaleOrder` → `string` | oOrder, string oStatusMsg | SQL: update: sales_orders,stock_movements ⚠ |
| `UpdateSalesorderSerial` | oClient, oWO, oItem, oSerial, oPallet, oTask, oActivity, oWarrantyNO, oMaker | SP: update_salesorder_serial |
| `UpdateSalesorderSerialHitachi` | oClient, oSO, oItem, oSerial, oPallet, oMaker, oCustomer, oLine | SP: update_salesorder_serial_hitachi |
| `UpdPickedMaster` | oOrderNo, oMarker | SP: pUpdataPickedMaster |
| `UpdStatusOrder` | oClient, oOrderNo | SP: D_UpdStatusOrder |

## Relocation / Inventory query / Stock list (59)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `check_pallet_loadsummary_check_in_store` | oClient, opallet_no, oloadsummary | SP: check_pallet_loadsummary_check_in_store |
| `CheckItemSerialPalletGet` | client, item_no, rec_num, pallet, serial_no | SP: Check_item_serial_pallet_get_rf |
| `CheckPalletInLocation` | pallet, warehouse, location, oType | SP: check_pallet_in_location |
| `CheckQty_maxPallet` | oClient, oItemNO, oUom, oQty | SP: get_equivalent_qty |
| `Confirm_StockAdj` | oRECNum, oNewUOM, oNewQTY, oDecFef, oMaker | SP: rf_update_pallet_location_qty |
| `ConfirmLocationRelocation` | oSRCWhse, oSRCLoc, oDestLoc, oMaker | SP: rf_location_relocation |
| `ConfirmPalletRelocation` | oSRCPallet, oDestWhse, oDestLoc, oDestGrade, oMaker | SP: rf_pallet_relocation |
| `Del_TransferPallet` → `bool` | oClient, oOrderNo | SP: D_del_TransferPallet |
| `Del_TransferPallet_line` → `bool` | oClient, oOrderNo, oPallet | SP: D_del_TransferPallet |
| `Generate_Location` → `bool` | oClient, oOrderNo | SP: D_Generate_Location |
| `Get_All_Pallet` | oPallet | SQL: select: pallet_location ⚠ |
| `Get_Barcode_Detail_pallet` | oClient, oBarcode | SQL: select: product_uom,product_master ⚠ |
| `get_check_pallet_st` | oLoc, pallet | SQL: select: pallet_location,locations,st_stock_count ⚠ |
| `get_CheckLocation` | oWH, oLocationScan | SQL: select: locations ⚠ |
| `Get_Data_Pallet_For_Replenishment` | oClient, oPallet, oBarcode | SP: get_data_pallet_for_replenishment |
| `Get_DetailLocInquiry` | client, location, warehouse | SP: HH_Location_detail |
| `Get_Exist_Pallet_Location` | oWarehouse, oPallet, oReceiptNO | SP: get_exist_pallet_location |
| `Get_Fine_Location` | oPallet | SP: rf_get_pallet_location |
| `Get_GradeInLocation` | oClient, oItem | SP: rf_get_grade |
| `Get_ItemLocationsGrade` | oClient, oItem, oGrade, oUom, oWH, oZone | SP: rf_get_item_locations_grade |
| `Get_ListItemInPalletLocation` | oClient, oItemNo | SQL: select: pallet_location,pallet_history ⚠ |
| `Get_ListItemInPalletLocation_ByItem` | oClient, oPallet, oBarcode | SQL: select: pallet_location,pallet_history,product_uom,product_master ⚠ |
| `Get_ListItemInPalletLocation_ByPallet` | oClient, oPallet | SQL: select: pallet_location,pallet_history,product_uom,product_master ⚠ |
| `Get_ListItemInPalletLocation_ByPallet_ByRecNum` | oClient, oPallet, oRecNum | SQL: select: pallet_location,pallet_history,product_master ⚠ |
| `Get_ListItemInPalletLocationByPalletRelocation` | oClient, oItemNo | SQL: select: pallet_location,pallet_history ⚠ |
| `Get_ListItemPallet_ByLocation` | oClient, oItemNo, oLocation | SQL: select: pallet_location,pallet_history,product_master ⚠ |
| `Get_ListPalletInLocation` | oWH, oLoc | SQL: select: pallet_location,pallet_history,product_uom,product_master ⚠ |
| `Get_Location_By_WH_Zone` | oClient, oWarehouse, oZone | SP: rf_showlocation_list |
| `get_location_itemscan` | listWhses, oLoc | SQL: select: locations ⚠ |
| `get_Location_stock_count_check` | oClient, oStk | SQL: select: stockcount_location_list ⚠ |
| `Get_LocationInquiry` | client, location, warehouse | SP: HH_Location_show |
| `Get_New_Pallet` | oClient | SP: next_pallet_no |
| `Get_Pallet` | oPallet | SQL: select: pallet_location ⚠ |
| `Get_Pallet_For_Replenishment` | oClient, oPallet | SP: get_pallet_for_replenishment |
| `Get_Pallet_Item` | oPallet | SQL: select: pallet_location ⚠ |
| `Get_Pallet_To_by_Works` | oWorksOrder | SP: get_pallet_to_by_works |
| `get_STK_Location` | oClient, oStk, oLoc, oWarehouse | SQL: select: st_stock_count,locations ⚠ |
| `getLocation` | oWarehouse | SQL: select: locations ⚠ |
| `getPalletStk` | oClient, oStk, oWarehouse, oLocation, oPallet | SP: get_pallet_st_stock_count |
| `GetTaskOutPallet` | client, warehouse, pallet | SQL: select: pallet_location ⚠ |
| `hh_packing_detail_update_count_pallet` | oClient, oPackingNo, oBarcode, oSalesOrder, oFlag_Check, oQty_Count, oMaker | SP: hh_packing_detail_update_count_pallet |
| `Item_Relocation` | oSrcPallet, oDestWhse, oDestLoc, oDestPallet, oClient, oItem, oSrcRec, oQty, oUOM, oMaker | SP: rf_item_relocation |
| `ItemSerialPalletGet` | client, item_no, rec_num, pallet | SP: item_serial_pallet_get_rf |
| `ItemSerialPalletUpdate` | client, item_no, serial_no, pallet, rec_num, maker | SP: item_serial_pallet_update |
| `Packing_Confirm_Pallet_Summary` | oClient, oPackingNo, oMaker, oSalesOrder, oPackingNoTarget | SP: hh_packing_confirm_pallet_summary |
| `Packing_Detail_Get_Detail_pallet` | oClient, oPackingNo, oSalesOrder | SP: hh_packing_detail_get_pallet |
| `Pallet_Location_Change_Update` | oClient, oReceiptNO, oWarehouse, oPallet, oLocation | SP: pallet_location_change_update |
| `Pallet_Relocation` | oWhseSource, oLocationI, oPalletI, oLocationII | SP: pallet_relocation |
| `Product_Prefer_Location_Get_ANJ` | oClient, oItem, oWarehouse, oLocation | SP: product_prefer_location_get_anj |
| `Product_Prefer_Location_Update_ANJ` | oClient, oItem, oWarehouse, oLocation, oMaker | SP: product_prefer_location_update_anj |
| `SelPalletMaster` | oPalletNo, oClient, oOrderNO | SP: D_sel_Pallet_Master |
| `Show_Sale_Pallet` | oPallet, oItemNo, oLotNo | SP: rf_show_pallet |
| `Show_StockList_Item` | oClient, oBarcode, oItemNo, oUom | SP: get_stock_list_by_ekapab |
| `Show_StockList_Location` | oClient, oWarehouse, oLocation | SP: get_stock_list_by_ekapab |
| `statusLocation` → `string` | oWarehouseFrom, oZoneFrom, oLocationFrom, oWarehouseDesc, oZoneDesc, oLocationDesc | SQL: select: locations ⚠ |
| `StockCountListLocation` | oClient, oWarehouse, oStockRef, oLocation | SQL: select: st_stock_count,locations ⚠ |
| `StockCountListLocation_store_branch` | oClient, oWarehouse, oStockRef, oLocation, oStore_branch | SQL: select: st_stock_count_branch,locations ⚠ |
| `UpdSalePallet` | oOrderNo, oLoading, oLine, oPalletNo, oQty | SP: rf_sale_pallet |
| `UpdSalePalletStatus` | oOrderNo, oLineNo | SP: updSaleDetailStatus |

## Stock count / Adjust (33)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `check_barcode_template_stock_count` | oClient, oTemplateNo, oWarehouse, oZone, oBarcode, oUsername, oStatus | SP: rf_ins_template_stockcount_detail |
| `check_create_stockcountbox` | oClient, oBranch | SP: check_create_stockcountbox |
| `CheckStockCounter` | oClient, oStockRef, oWarehouse, oLocation, oPallet, oUsername | SP: check_stock_counter |
| `Close_StockCountBox_store_branch` | oClient, oBranch, oDoc_No, oMaker | SP: close_stockcountbox_doc |
| `closed_stock_count_store_branch` | oClient, oStockRef, oStore_branch, oMaker | SP: closed_stock_count_store_branch |
| `CreateStoreStockCountBoxHeader` | oClient, oBranch, oMaker | SP: create_stockcountbox_header |
| `Delete_Stock_Count` | oSttRef, oRecNum, oWh, OLoc, oPallet, oClient, oItem, oUOM, oGrade, oMaker | SP: delete_stock_count |
| `DeleteStockcountboxDetail` | oClient, oDoc_no, oBranch, oItem_no, oBarcode, oMarker | SP: delete_stockcountbox_detail |
| `get_template_stock_count_detail` | oClient, oTemplateNo, oWarehouse, oZone | SP: rf_ins_template_stockcount_detail_show |
| `GetStockCount` | oClient, oStockRef, oWarehouse, oLocation, oPallet, oItem | SP: get_stock_count |
| `GetStockCount_store_branch` | oClient, oStockRef, oWarehouse, oLocation, oStore_branch, oMaker | SP: get_stock_count_store_branch_new |
| `GetStockCount_store_branch_item` | oClient, oStockRef, oWarehouse, oLocation, oStore_branch, oMaker, oItem, oBarcode | SP: get_stock_count_store_branch_item |
| `GetStockCountBoxDetail` | oClient, oBranch, oDoc_no | SP: get_stockcountbox_detail |
| `getStoreStockCountBoxList` | oClient, oBranch, oStartDate, oEndDate | SP: web_get_stockcountbox_list |
| `GetTemplateNo` | oClient | SQL: select: template_stockcount_master ⚠ |
| `reset_stock_count_item_store_branch` | oClient, oStockRef, oStore_branch, oMaker, oItem, oBarcode | SP: reset_stock_count_item_store_branch |
| `Save_Template_Stock_Count_Header` | oClient, oTemplateNo, oStatus, oWarehouse, oZone, oRemarks, oMaker | SP: rf_ins_template_stockcount_header |
| `StockAdjustmentUpdateStockcountNew` | oRecNum, oClient, oWH, oLocation, oPallet, oItemNo, oLot, oBatch, oExpDate, oProdDate, oSize, oColor, oClass, oOwner, oGrade, oMaker, oUOM, oQty, oItemBarcode | SP: stock_adjustment_update_stockcount_new |
| `StockAdjustmentUpdateStockcountNewWindows` | oRecNum, oClient, oWH, oLocation, oPallet, oItemNo, oLot, oBatch, oExpDate, oProdDate, oSize, oColor, oClass, oOwner, oGrade, oMaker, oUOM, oQty, oItemBarcode | SP: stock_adjustment_update_stockcount_new |
| `StockCountRefGet` | oClient, oCount_type | SP: stock_count_ref_get |
| `StockCountRefGet_store_branch` | oClient, oCount_type, oWH, oStore_branch | SP: stock_count_branch_ref_get |
| `SummaryStockCount` | oClient, oStockRef, oWarehouse, oLocation, oPallet, oItem, oMaker | SP: summary_stock_count |
| `SummaryStockCountEdit` | oClient, oStockRef, oWarehouse, oLocation, oPallet, oItem, oMaker | SP: summary_stock_count_edit |
| `template_stockcount_detail_delete` | oClient, oTemplateNo, oWarehouse, oBarcode, oUsername | SP: rf_ins_template_stockcount_detail_delete |
| `update_devalue_stock_count_store_branch` | oSttRef, oRecNum, oWh, OLoc, oPallet, oClient, oItem, oItemBarcode, oUOM, oSQty, oCQty, oGrade, oLotNo, oBatNo, oExpDate, oProdDate, oSize, oColor, oClass, oOwner, oMaker, oStore_branch | SP: update_devalue_stock_count_store_branch |
| `Update_Stock_Count` | oSttRef, oRecNum, oWh, OLoc, oPallet, oClient, oItem, oItemBarcode, oUOM, oSQty, oCQty, oGrade, oLotNo, oBatNo, oExpDate, oProdDate, oSize, oColor, oClass, oOwner, oMaker, oRecord_no | SP: update_stock_count |
| `Update_Stock_Count_Confirm` | oClient, oStock_Ref, oWarehouse, oLocation, oItem, oMaker | SP: update_stock_count_confirm0 |
| `Update_Stock_Count_Confirm_new` | oClient, oStock_Ref, oWarehouse, oLocation, oItem, oMaker, oPallet, oRecord_no | SP: update_stock_count_confirm0_new |
| `Update_Stock_Count_ConfirmEnd` | oClient, oStk, oLoc, oWarehouse | SQL: update: st_stock_count ⚠ |
| `Update_Stock_Count_ConfirmEnd_N` | oClient, oStk, oLoc, oWarehouse | SQL: update: st_stock_count ⚠ |
| `Update_Stock_Count_store_branch` | oSttRef, oRecNum, oWh, OLoc, oPallet, oClient, oItem, oItemBarcode, oUOM, oSQty, oCQty, oGrade, oLotNo, oBatNo, oExpDate, oProdDate, oSize, oColor, oClass, oOwner, oMaker, oStore_branch | SP: update_stock_count_store_branch |
| `UpdatestockcountboxDetailStoreBranch` | oClient, oDoc_no, oBranch, oItem_no, oBarcode, oUom, oQty, oMarker, oFlag | SP: update_store_stockcountbox_detail |
| `UpdateStockcountboxHeader` | oClient, oDoc_no, oBranch, oOrderNo, oMarker, oRemarks | SP: update_stockcountbox_header |

## Replenishment (7)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `Close_Replenish_Onestep_New` | oWoNo, oSoNo, oLineNo, oRecNum, oClient, oItemNo, oWarehouse, oLocation_Fr, oPallet_Fr, oLocation_To, oBarcodeFr, oBarcodeTo, oUomFr, oQtyFr, oUomTo, oQtyTo, oMaker, oPalllet_Dest | SP: close_replenish_onestep_new |
| `Closed_Replenishment` | oRecNum, oClient, oItem, oWarehouse, oLocationFR, oPallet, oLocationTO, oBarcodeFR, oBarcodeTO, oQtyFR, oUomFR, oQtyTO, oUomTO, oMaker, oPalllet_Dest | SP: close_replenish |
| `Closed_Replenishment_New` | oWO, oSalesOrder, oTaskNo, oActivity, oLocationTO, oBarcodeTO, oQtyFR, oUomFR, oQtyTO, oUomTO, oMaker | SP: close_replenish_new |
| `Get_Data_Barcode_For_Replenishment` | oClient, oBarcodeFr, oQty, oUom, oBarcodeTo | SP: get_data_barcode_for_replenishment |
| `Get_Loaction_for_Replenish` | oWarehouse, oZone, oLocation | SQL: select: locations ⚠ |
| `GetItemListForReplenishment` | oClient, oItemNo, oBarcode, oWarehouse, oZone, oAisle | SP: get_item_list_for_replenishment |
| `Reverse_Trx_Replenishment_by_Task` | oClient, oSalesOrder, oWorkOrder, oTask, oActivity, oMaker | SP: reverse_trx_replenishment_by_task |

## Packing / Load to truck / Delivery / Jobs (49)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `Check_Loading_Item_Packing_Qty` | oClient, oLoadingSummaryNo, oPackingNo, oItemBarcode, oReceiptType | SP: check_loading_item_packing_qty |
| `Check_Loading_No` | oClient, oLoading_no | SP: check_loading_no |
| `Check_Pack_No` | oClient, oLoading_no, oPack_no | SP: check_pack_no |
| `Check_Packing_Count` | oClient, oPackingNo, oBarcode, oSalesOrder | SP: hh_check_packing_count |
| `Close_Box` | List<string[]> parameters | SP: insupd_packing_detail |
| `Confirm_Loading_Summary_Detail` | oLoadingSummaryNo, oMaker | SP: close_loadsummary_and_update_pack |
| `confirm_pack` → `void` | List<string[]> parameters | SQL: update: packing_order_detail,packing_order |
| `Confirm_Return_Pack` | oClient, oLoad_no | SP: comfirm_return_pack |
| `deleteDeliveryPicture` | oClient, oLoadNo | SQL: delete: delivery_picture ⚠ |
| `DownloadFile` → `byte[]` | - | SQL: update: HH |
| `get_equipment` | - | SQL: select: equipment |
| `Get_Load_Summary_No_Check` | oClient, oLoadingSummaryNo, oBranch | SP: web_get_load_summary_no |
| `Get_Loading_Detail` | oClient, oLoadingNo | SQL: select: packing_order ⚠ |
| `Get_Loading_Detail_All` | oClient, oLoadingNo | SQL: select: packing_order,customer_master ⚠ |
| `Get_Loading_Detail_close` | oClient, oLoadingNo | SQL: select: packing_order ⚠ |
| `Get_Loading_Detail_Return` | oClient, oLoadingNo | SQL: select: packing_order ⚠ |
| `Get_Loading_List` | oClient, oLoad_no | SQL: select: packing_order ⚠ |
| `Get_Loading_Summary_Detail` | oLoadingSummaryNo, oClient | SP: get_loading_summary_detail |
| `Get_Loading_Summary_Detail_Item_List` | oLoadingSummaryNo | SP: get_loading_summary_detail_item_list |
| `Get_Loading_Summary_List` | - | SP: get_loading_summary_list |
| `Get_Loading_Summary_Truck_Driver` | - | SP: get_loading_summary_truck_driver |
| `Get_Loading_Summary_Truck_Type` | - | SP: get_loading_summary_truck_type |
| `get_loadsummary` | oClient, oBranch, oDocRef | SP: get_loadsummary |
| `Get_Packing_Carton` | client, packing_no | SQL: select: packing_order_detail ⚠ |
| `Get_Packing_no` | client, customer | SQL: select: packing_order ⚠ |
| `get_packing_no_new` | oClient, oPack_No | SP: get_packing_no_new |
| `GetDelivery` | oClient, oDelivery | SP: rf_get_delivery |
| `GetDeliveryALL` | oClient, oDelivery | SP: rf_get_delivery_All |
| `getDeliveryPicture` | oClient, oLoadNo, oPackNo, oPicture_no | SQL: select: delivery_picture,packing_order ⚠ |
| `InsertPrintJob` | client, warehouse, did, user, reportname, List<string[]> parameters, copyqty | SP: printjob_print_job_master, printjob_print_job_details |
| `insupdTRANSFERDETAIL` → `bool` | oClient, oOrderNo, oPalleNO, oJournalName, oMaker | SP: D_insupd_D_TRANSFER_DETAIL |
| `Loadsummary_Item_Not_Load` | oLoadingSummaryNo | SP: rpt_loadsummary_item_not_load |
| `Packing_Detail_Check_Print` | oClient, oPackingNo, oMaker, oSalesOrder | SP: hh_packing_detail_check_print |
| `Packing_Detail_Confirm_Count` | oClient, oPackingNo, oMaker, oSalesOrder | SP: hh_packing_detail_confirm_count_and_packed |
| `Packing_Detail_Get_Detail` | oClient, oPackingNo, oSalesOrder | SP: hh_packing_detail_get |
| `Packing_Detail_Get_Header` | oClient, oPackingNo | SP: hh_packing_header_get |
| `Packing_Detail_Reset_Count` | oClient, oPackingNo, oMaker, oSalesOrder | SP: hh_packing_detail_reset_count |
| `Packing_Detail_Reset_Count_item` | oClient, oPackingNo, oMaker, oSalesOrder, oBarcode | SP: hh_packing_detail_reset_count_by_item |
| `Packing_Detail_Update_Count` | oClient, oPackingNo, oBarcode, oSalesOrder, oFlag_Check, oQty_Count | SP: hh_packing_detail_update_count |
| `QueryAllNotMSG` | SaleOrder | SP: get_loadref_do_ALL_BY_DC |
| `reportPrintJob` → `bool` | - | SQL: update: HH |
| `Save_Loading_Summary_Header` | oLoadingSummaryNo, oStatus, oDeliveryDate, oVehicle, oVehicleType, oDriver, oMaker, oDriver2 | SP: get_loadsummaryNo, update_loadsummary_master |
| `selectPrintJob` | - | SP: select_print_job |
| `selectPrintJobData` | PrintNo | SQL: select:  |
| `SelTRANSFER` | oClient, oOrderNO | SP: D_sel_TRANSFER |
| `SelTransferDetail` | oClient, oOrderNo | SP: D_sel_TransferDetail |
| `Upd_SIPacked` → `void` | List<string[]> parameters | SQL: update: sales_details |
| `update_delivery_confirmation` | oClient, oSalesOrder, oDelivery, oDelivery_product_date, oDelivery_product_time, oDate_Product_Invoice, oInvoice, oRecipientID, oRecipient, oRemark, oRemark_work, oStatus_work | SP: update_delivery_confirmation |
| `Update_Packing_Flag` | oClient, oLoadno, oPack_no, oPicture_no | SP: update_packing_flag |

## Goods issue / Serial (17)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `clear_TempGoodsIssue` | maker, order_no | SP: sp_cleartempgoodsissue |
| `ClosedSoSerial` | oClient, oSO | SP: closed_so_serial |
| `create_GoodsIssue_Lines` | client, order_no, maker, whse_source, zone, draft_maker | SP: sp_create_goodsissue_lines |
| `delete_GoodsIssue_Detail` | client, order_no, line_no | SP: sp_delete_goodsissue_detail |
| `delete_TempGoodsIssue` | item_no, maker, order_no | SP: sp_deletetempgoodsissue |
| `DeleteSerialNo` | oClient, oItem, oReceipt, oSerial, oMaker | SP: delete_serial_no |
| `GenSerial` | client, item, receipt, line, receipt_date, pallet, qty, user | SP: gen_serial_no |
| `get_GoodsIssue_Detail` | client, order_no | SP: sp_get_goodsissue_detail |
| `get_TempGoodsIssue` | maker, order_no | SP: sp_gettempgoodsissue |
| `GetSoDetailSerial` | oClient, oSO | SP: get_sodetail_serial |
| `GetSoSerial` | oClient, oSO | SP: get_so_serial |
| `insert_TempGoodsIssue` | item_no, qty, uom, order_no, maker | SP: sp_inserttempgoodsissue |
| `postStockMovement` → `string` | oDT, string ostatusmsg, string oMessage | SP: insert_temp_stock_movement |
| `ProductSerialGet` | oClient, oItem, oReceipt, oPallet | SP: product_serial_get |
| `SerialFlagCheckAdjus` | client, item_no, rec_num | SP: serial_flag_check_adjus |
| `SerialPendingGet` | oClient, oItem, oReceipt, oPallet, oQty | SP: serial_pending_get |
| `update_TempGoodsIssue` | item_no, qty, maker, order_no | SP: sp_updatetempgoodsissue |

## Print / PDF / Report / File (1)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `updatePrintStatus` | PrintNo | SP: update_print_status |

## Lookups / Other (128)

| Endpoint | พารามิเตอร์ | ที่มา |
|---|---|---|
| `assignWhseTaskIW` | oMaker, oMovementType, oClient, oOrderNo | SP: assign_whse_task_manual_IW |
| `Cal_Eff_Unit_Cost` | oClient, oReceiptNo | SP: cal_eff_unit_cost |
| `check_loansummary_receitp` | oClient, oBranch, oRef, oReceiptNo | SP: check_loansummary_receitp |
| `Check_Product_Tag` | oClient, oBarcode | SP: check_product_tag |
| `check_updateORinsert` | oClient, oTemplateNo | SQL: select: template_stockcount_master ⚠ |
| `checkBook` → `bool` | oBook | SQL: select: books_master ⚠ |
| `checkClient` → `bool` | oClient | SQL: select: client_master ⚠ |
| `Chk_Active_WH` | User, Client, Zone | SQL: select: product_zoning,user_group_access_control,zones ⚠ |
| `ChkStUnitCountByRecNum` | oClient, oWarehouse, oStk_ref, oRec_num, oLoc, oMaker | SQL: select: st_stock_unit_count ⚠ |
| `Close_Sale_Return` | oClient, oBranch, oDoc_No, oMaker | SP: web_close_branch_return_doc |
| `Close_Sale_Return_store_branch` | oClient, oBranch, oDoc_No, oMaker | SP: web_close_store_branch_return_doc |
| `conItemNoToItemCode` → `string` | oitem_no | SQL: select: product_master ⚠ |
| `conSupNoToSupName` → `string` | oSupNo | SQL: select: suppliers ⚠ |
| `CreateBranchReturnHeader` | oClient, oBranch, oMaker | SP: web_create_branch_return_header |
| `del_barcode_description` | oBarcode_no, oMaker | SP: del_barcode_description |
| `delect_st_stock` | oClient, oLoc, oItem, oPallet, oStk, type | SP: rf_delect_st_stock |
| `DeleteSaleReturnDetail` | oClient, oDoc_no, oBranch, oLine_no, oItem_no, oUom, oQty, oRemark, oSource, oMarker | SP: web_delete_branch_return_detail |
| `DeleteSaleReturnDetailStoreBranch` | oClient, oDoc_no, oBranch, oLine_no, oItem_no, oUom, oQty, oRemark, oSource, oMarker | SP: web_delete_store_branch_return_detail |
| `DoDeleteUnitCount` | oRecNum, oStk, oLoc | SP: delete_st_stock_unit_count |
| `Gen_carton` | client, customer | SQL: exec:  |
| `generate_number` | - | SP: gen_number |
| `Get_Aisle` | oWarehouse, oZone | SP: get_aisle |
| `Get_All_Product_Master` | oClient | SQL: select: product_master,product_uom ⚠ |
| `Get_AllItems` | oClient | SQL: select: product_master ⚠ |
| `Get_AllUom` | - | SQL: select: unit_of_measure |
| `get_barcode_description` | oBarcode_no | SP: get_barcode_description |
| `Get_Barcode_Detail` | oClient, oBarcode, oItemNo | SQL: select: product_uom,product_master ⚠ |
| `get_Barcode_Detail_Stock` | oClient, oBarcode, oItem | SQL: select: product_uom,product_master,receipt_detail ⚠ |
| `Get_BarcodeMaster_Ekapab` | oClient, oBarcode | SP: get_barcode_master_by_ekapab |
| `Get_Branch` | oClient | SQL: select: user_profile ⚠ |
| `Get_Branch01` | oClient | SQL: select: customer_master ⚠ |
| `get_branchs` | - | SQL: select: branchs_master |
| `Get_Brand` | oClient | SP: rf_showbrand_list |
| `Get_Client` | User | SQL: select: client_master,user_group_access_control,user_profile ⚠ |
| `Get_Client_client` | User, Client | SQL: select: client_master,user_group_access_control,user_profile ⚠ |
| `Get_ClientByUser` | oUser | SQL: select: user_group_access_control ⚠ |
| `Get_Customer_By_SI` | oClient, oSalesOrder | SP: hh_get_customer_by_si |
| `Get_Customer_Master` | oClient, oCustomer | SQL: select: customer_master ⚠ |
| `Get_DesByZone` | zone | SQL: select: zones ⚠ |
| `Get_DescUom` | oUom | SQL: select: unit_of_measure ⚠ |
| `get_doGetDataSaveDetail` | oClient, oReceipt | SQL: select: receipt_detail,product_master ⚠ |
| `Get_ExpiryDateByItem` | oClient, oItemNo, oMFG | SQL: select: product_master ⚠ |
| `Get_Grade` | - | SQL: select: parameter_tbl |
| `Get_Grade_tbl` | - | SQL: select: parameter_tbl |
| `Get_Inbox` | client, packing_no, carton | SQL: select: packing_order_detail ⚠ |
| `Get_Item` | oClient, oItem | SP: get_matching_product2 |
| `Get_Item_Barcode` | oClient, oBarcode | SP: rf_sel_item_barcode |
| `get_item_barcode` | oSearch, oClient | SQL: select: product_uom,product_master ⚠ |
| `Get_Item_Barcode_CheckPrice` | oClient, oBarcode, oMaker, oBranch | SP: rf_sel_item_barcode_check_price |
| `Get_Item_Description_Branch` | oClient, oItemNo | SQL: select: product_master,product_uom ⚠ |
| `Get_Item_Detail_Branch_for_countbox` | oClient, oItemNo | SQL: select: product_master,product_uom ⚠ |
| `Get_Item_No_By_WH_And_Zone` | oClient, oWarehouse, oZone | SP: rf_showproduct_list |
| `Get_Item_Tranfer_Detail` | oClient, oItem, oMaker | SP: get_detail_item_for_web |
| `Get_ItemDesc` | oItem | SQL: select: product_master ⚠ |
| `Get_ItemListGrade` | oClient, oItem | SP: rf_get_item_list_grade |
| `Get_ItemListUOM` | oClient, oItem | SP: rf_get_item_list_uom |
| `Get_ItemNo` | oClient, oItemNO | SQL: select: product_master ⚠ |
| `Get_Loaction` | oClient, oLocation | SQL: select: locations,product_zoning ⚠ |
| `Get_Loaction_all` | oWarehouse, oLocation | SQL: select: locations ⚠ |
| `Get_Loaction_Warehouse` | oClient, oItemNO, oWH, oLOC | SQL: select: locations,product_zoning ⚠ |
| `Get_Loaction_Zone_Warehouse` | oClient, oItemNO, oWH, oLOC_DESC, oLOC | SQL: select:  ⚠ |
| `Get_MapItem` | oBarcode | SQL: select: product_barcode ⚠ |
| `Get_MatchProduct` | oClient, oItem | SP: get_matching_product2 |
| `Get_Picture` | - | SQL: select: product_master,delivery_picture |
| `Get_ProductOther` | oClient, oItemNo | SQL: select: product_master ⚠ |
| `Get_ProductUom` | oClient, oItem | SP: rf_get_product_uom_2_level |
| `get_qty_total_directostore` | oClient, oReceiptNo | SP: get_qty_total_directostore |
| `Get_SI` | client, customer | SQL: select: sales_orders ⚠ |
| `Get_SI_detail` | client, order_no | SQL: select: sales_details ⚠ |
| `Get_SO_From_WO` | order_no | SQL: select: stock_movements ⚠ |
| `Get_Stock_For_Stockadjus` | oClient, oWH, oLocation, oPallet, oItemNo | SP: get_stock_for_stockadjus |
| `Get_Supplier` | oClient | SQL: select: suppliers ⚠ |
| `Get_TopClient` | User | SQL: select: client_master,user_group_access_control,user_profile ⚠ |
| `Get_UOM` | oClient, oItemNo | SQL: select: product_uom ⚠ |
| `Get_UOM_Branch` | oClient, oItemNo | SQL: select: product_uom,uom_master ⚠ |
| `Get_UOM_Branch_Return` | oClient, oItemNo | SQL: select: product_uom ⚠ |
| `Get_UOM_By_Item` | oClient, oItemNo | SQL: select: product_uom ⚠ |
| `Get_Uom_Code` | oClient, oItem, oItem_Packing | SQL: select: product_uom ⚠ |
| `Get_Warehouse` | oUser | SQL: select: warehouses,user_group_access_control ⚠ |
| `Get_WarehouseByUser` | user | SQL: select: user_group_access_control,warehouses ⚠ |
| `Get_WarehouseLoc` | - | SQL: select: warehouses |
| `Get_wh` | oUser | SQL: select: user_group_access_control ⚠ |
| `Get_WhLoc` | oPallet | SQL: select: pallet_location ⚠ |
| `Get_ZoneByUser` | warehouse, user | SQL: select: user_group_access_control,zones ⚠ |
| `Get_ZoneByWH` | warehouse | SQL: select: zones ⚠ |
| `getBranchReturnHeader` | oClient, oBranch, oDoc_no | SP: web_get_branch_return_header |
| `getBranchReturnList` | oClient, oBranch, oStartDate, oEndDate | SP: web_get_branch_return_list |
| `GetClientWarehouse` | oClient | SP: get_client_warehouse |
| `GetImageProduct` | client, item_no | SQL: select: product_master ⚠ |
| `getReasonReturnCode` | - | SP: web_get_reason_return_code |
| `GetSaleReturnDetail` | oClient, oBranch, oDoc_no | SP: web_get_branch_return_detail |
| `getSiDetail` | oClient, oBook, oOrder_no | SQL: select: sales_details,sales_orders,product_master ⚠ |
| `GetStockSumUnitCount` | oClient, oStockRef, oWarehouse, oLocation, oPallet, oItem, oMaker | SP: get_stock_sum_unit_count |
| `GetStockUnitCount` | oClient, oStockRef, oWarehouse, oLocation, oPallet, oItem, oMaker | SP: get_stock_unit_count |
| `getStoreBranchReturnList` | oClient, oBranch, oStartDate, oEndDate | SP: web_get_store_branch_return_list |
| `GetSupplier` | oClient, oSupplier | SP: rf_get_supplier |
| `GetTaskOutSOItem` | client, warehouse, pallet, item_no | SQL: select: from ⚠ |
| `GetUOMlose` | oClient, oItemNO | SQL: select: product_uom ⚠ |
| `InsertBarcode` | List<string[]> parameters | SP: insert_fix_scan |
| `insup_barcode_description` | oBarcode_no, oWidth, oDepth, oHight, oWeight, oMaker | SP: insup_barcode_description |
| `Map_Barcode` | oClient, oBarcode, oItem, oRemark, oMaker, oStatus, oMsg | SP: rf_update_product_barcode |
| `PreConfirmationTaskCheck` | oWO, oPallet, oTask, oActivity, oUom, oQty, oMaker | SP: pre_confirmation_task_check |
| `RegisterDID` | did | SP: ins_did |
| `Reverse_Trx_SIbyTask_by_Line` | oClient, oSalesOrder, oWorkOrder, oLine, oMaker | SP: reverse_trx_sibytask_new_by_line |
| `rf_check_assigned` | oClient, maker, wo | SP: rf_check_assigned |
| `rfCcUpdateCycleCount` | oMaker, oWh, OLoc, oPallet, oClient, oItem, oUOM, oSQty, oCQty, oGrade, oSttRef, oRecNum, oLotNo, oBatNo, oExpDate, oProdDate, oSize, oColor, oClass, oOwner, oRemarks | SP: rf_cc_update_cycle_count |
| `RunNUMBER` | oDOCType | SP: D_RunNumber |
| `SelClientProfile` | oClient | SP: D_sel_ClientProfile |
| `setDefaultClient` | user_id | SQL: select: client_master,user_group_access_control ⚠ |
| `setJournal` | type, journal | SQL: select: JournalName ⚠ |
| `SetStock` | oClient, oWarehouse, oLocation | SP: rf_set_stock |
| `Show_Sale_Item_List` | oOrder, oLoading | SP: rf_show_item_list |
| `showBook` | - | SQL: select: books_master |
| `showBookAJ` | listBranch | SQL: select: books_master ⚠ |
| `showBookGR` | listBranch | SQL: select: books_master ⚠ |
| `showBookOT` | oWH | SQL: select: books_master,branchs_master ⚠ |
| `showBookRT` | - | SQL: select: books_master |
| `showBookTR` | oWH | SQL: select: books_master,warehouses ⚠ |
| `ShowClient` | - | SP: rf_sel_cilent |
| `ShowItemList` | oClient, oReceiptNO, oPONo, oItemNO | SP: rf_sel_item_list |
| `shu_sel_product_pictures` | oClient, oItem | SP: shu_sel_product_pictures |
| `TopClient` | User | SQL: select: client_master,user_group_access_control,user_profile ⚠ |
| `Update_Sale_Detail` | oClient, oWo, oOrder, oItem, oQty, oUom, oLine, oZone | SP: update_sale_detail |
| `Update_Stock_Unit_Count` | oSttRef, oRecNum, oWh, OLoc, oPallet, oClient, oItem, oItemBarcode, oUOM, oSQty, oCQty, oGrade, oLotNo, oBatNo, oExpDate, oProdDate, oSize, oColor, oClass, oOwner, oMaker, oRecord_no | SP: insert_stock_unit_count |
| `UpdateSaleReturnDetail` | oClient, oDoc_no, oBranch, oLine_no, oItem_no, oUom, oQty, oRemark, oSource, oMarker, oReason_code | SP: web_update_branch_return_detail |
| `UpdateSaleReturnDetailStoreBranch` | oClient, oDoc_no, oBranch, oLine_no, oItem_no, oUom, oQty, oRemark, oSource, oMarker, oReason_code | SP: web_update_store_branch_return_detail |
| `updateStatusLineSale` → `void` | oOrder, oLineNo | SQL: update: sales_details ⚠ |
| `UpdProductUom` | oClient, oItem, oItemUom, oItemHeight, oItemWidth, oItemDepth, oItemWeight, oPalletWidth, oPalletDepth, oPalletQty, oMaker | SP: rf_update_product_uom_2_level |
