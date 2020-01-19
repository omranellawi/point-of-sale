<?xml version="1.0" encoding="UTF-8"?>
<templates id="point_of_sale.template" xml:space="preserve">
    <t t-extend="PosTicket" align='center' width='48' t-as="doc">
        <t t-jquery='.pos-sale-ticket' t-operation='replace' >
            <div  class="pos-sale-ticket" font-size="20px">
                <div>
                   <center> <t t-if='receipt.company.logo'>
                <img t-att-src='receipt.company.logo' style=" height: 200px;width: 80%;" />
                <br/>
                  </t></center>
                    <t  t-if='!receipt.company.logo'>
                    <t t-esc='receipt.company.name' />
                <br/>
                         </t>
<!---->
<!---->
                </div>
                <font face="verdana">
                <div >
                <t t-if='receipt.company.contact_address'>
                    <div><t t-esc='receipt.company.contact_address' /></div>
                </t>
                <t t-if='receipt.company.phone'>
                    <div>Tel:<t t-esc='receipt.company.phone' /></div>
                </t>
<!--                <t t-if='receipt.company.vat'>-->
<!--                    <div>VAT:<t t-esc='receipt.company.vat' /></div>-->
<!--                </t>-->
                <t t-if='receipt.company.email'>
                    <div><t t-esc='receipt.company.email' /></div>
                </t>
                <t t-if='receipt.company.website'>
                    <div><t t-esc='receipt.company.website' /></div>
                </t>
                <t t-if='receipt.header_xml'>
                    <t t-raw='receipt.header_xml' />
                </t>
                <t t-if='!receipt.header_xml and receipt.header'>
                    <div><t t-esc='receipt.header' /></div>
                </t>
                <t t-if='receipt.cashier'>
                    <div class='cashier'>
                        <strong><div>--------------------------------</div></strong>
                        <div><strong>Served by : <t t-esc='receipt.cashier' /></strong></div>
                    </div>

                </t>
            </div>
                </font>
                <br/>

                <br />
                 <font face="verdana">
                <div>
                <t t-if='receipt.client'>
                  <div>Customer : <t t-esc='receipt.client' /></div>
              </t>
          </div>
                 </font>
<!--                    Qty    U.P        Amount</pre></div>-->
                 <font face="verdana">
          <line line-ratio='1'><strong><left>------------------------------------------</left></strong></line>

                <t t-if="receipt.header">
                    <div style='text-align:center'>
                        <t t-esc="receipt.header" />
                    </div>
                    <br/>
                </t>

                <div class='orderlines' line-ratio='0.6'>
            </div>
                                <table class='receipt-orderlines'>
                    <colgroup>
                        <col width='60%' />
                        <col width='30%' />
                        <col width='58%' />
                        <col width='30%' />
                    </colgroup>
                    <tr style="border: 0px solid rgb(0, 0, 0);">
                        <th> <strong>Name</strong></th>
                        <th><strong>Qty</strong></th>
                        <th><strong>Price</strong></th>
                        <th><strong>TOTAL</strong></th>
                    </tr>
                    <tr t-foreach="orderlines" t-as="orderline">
                        <td>
                            <t t-esc="orderline.get_product().display_name"/>
                             <t t-if="orderline.get_discount() > 0">
                                <div class="pos-disc-font">
                                    With a <t t-esc="orderline.get_discount()"/>% discount
                                </div>
                            </t>
                        </td>
                        <td>
                            <t t-esc="orderline.get_quantity_str_with_unit()"/>
                        </td>
                        <td>
                            <t t-set="a" t-value="orderline.quantityStr"></t>
                            <t t-set="b" t-value="orderline.get_display_price()"></t>
                            <t t-set="c" t-value="b/a"></t>
                            <t t-esc="c"/>
                        </td>
                        <td style='text-align:right'>
                            <t t-esc="widget.format_currency(orderline.get_display_price())"/>
                        </td>
                    </tr>
                </table>
                 </font>
<!---->
                <br />
                 <font face="verdana">
            <t t-set='taxincluded' t-value='Math.abs(receipt.subtotal - receipt.total_with_tax) &lt;= 0.000001' />
            <t t-if='!taxincluded'>
                <line line-ratio='1'><left>------------------------------------------------</left></line>
                <line ident="3">
<!--                    <left></left>-->
                    <right><strong>Sub-total<t t-esc="receipt.subtotal" /></strong></right>
                </line>
                           </t>
                <br/>
<!--  Total -->
            <line line-ratio='1'><strong><left>------------------------------------------</left></strong></line>
            <line class='total' size='double-height'>
<!--                <left><pre>TOTAL</pre></left>-->
                <right> <strong>TOTAL : <t t-esc='receipt.total_with_tax' /> KWD </strong></right>
            </line>
                <br/>
                <br/>
<!---->
<!--           Payment Lines -->
            <t t-foreach='paymentlines' t-as='line'>
                <line>
<!--                    <left></left>-->
                    <right><strong> <t t-esc='line.name' />  : <t t-esc='line.get_amount()'/>  KWD</strong></right>
                </line>
            </t>
                <br/>
                <br/>
            <line>
<!--                <left><pre>CHANGE</pre></left>-->
                <right><strong>CHANGE :<t t-esc='receipt.change' />  KWD</strong></right>
            </line>
            <br/>
            <br/>

<!--           Extra Payment Info -->
            <t t-if='receipt.total_discount'>
                <line>
<!--                    <left>Discounts</left>-->
                    <right><strong> Discounts :<t t-esc='receipt.total_discount'/>  KWD  </strong></right>
                </line>
            </t>
                <br/>

          <!-- Footer -->
            <t t-if='receipt.footer_xml'>
                <t t-raw='receipt.footer_xml' />
            </t>

            <t t-if='!receipt.footer_xml and receipt.footer'>
                <br/>
                <t t-esc='receipt.footer' />
                <br/>
                <br/>
            </t>

            <div class='after-footer' />

            <br/>
           <center><div font-size='20px'>
                <div><strong><t t-esc='receipt.name' /></strong></div>
                <div><strong><t t-esc='receipt.date.localestring' /></strong></div>
            </div></center>
                 </font>
                <br/>
            </div>
        </t>
    </t>
</templates>
