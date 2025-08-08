```verilog
`timescale 1ns/1ps
module tb_jk_ff_sync_rst_n;
  reg j,k,clk,rst_n;
  wire q, q_bar;
  
  jk_ff_sync_rst_n DUT (j,k,clk,rst_n,q,q_bar);
  
  always #2 clk = ~clk;
  
  initial begin
  clk = 0;
  rst_n = 0;
  j = 0;
  k = 0;
    $display ("Reset=%b ---> q=%b|q_bar=%b",rst_n,q,q_bar);
  
  #3 rst_n = 1;
    $display ("Reset=%b ---> q=%b|q_bar=%b",rst_n,q,q_bar);
  #3 drive(2'b10);
  #3 drive(2'b01);
  #3 drive(2'b10);
  #3 drive(2'b00);
  #3 drive(2'b11);
  
  #5;
  $finish;
  end
  
  task drive(bit[1:0]ip);
    @ (posedge clk) begin
    {j,k} = ip;
      #1
      $display("j=%b ,k=%b ,q=%b ,q_bar=%b",j,k,q,q_bar);
    end
  endtask
  
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
endmodule
