```verilog
module jk_ff_sync_rst_n (input j ,k ,clk ,rst_n,
                         output reg q,
                         output wire q_bar);
  
  // always @ (posedge clk or nagedge rst_n); - Asynchronous
  always @ (posedge clk) begin
    if(rst_n ==0)
      q <= 0;
  else case ({j,k})
    2'b00: q <= q;
    2'b01: q <= 0;
    2'b10: q <= 1;
    2'b11: q <= q_bar;
    default: q <= 1'bx;
  endcase
  end
  
    assign q_bar = ~q;
endmodule
   
