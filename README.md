# CompArchLab2
module FSM3(I,clock,reset,F,st_out);
input clock; // clock signal
input reset; // reset input
input I; // binary input
output reg F; // output of the sequence detector
output reg [2:0]st_out;

parameter  S0=3'b000, // "S0" State
  S1=3'b001, // "S1" State
  S2=3'b011, // "S2" State
  S3=3'b010, // "OnceS0S1" State
  S4=3'b110;// "S4" State
  
reg [2:0] state, next_state; // current state and next state

// sequential memory of the Moore FSM
always @(posedge clock, posedge reset)
begin
 if(reset==1) 
 state <= S0;// when reset=1, reset the state of the FSM to "S0" State
 else
 state <= next_state; // otherwise, next state
end 

// combinational logic of the Moore FSM
// to determine next state 


always @(state,I)
begin
 case(state) 
 S0:begin
  if(I==1)
   next_state = S1;
  else
   next_state = S0;
 end
 S1:begin
  if(I==0)
   next_state = S2;
  else
   next_state = S1;
 end
 S2:begin
  if(I==0)
   next_state = S3;
  else
   next_state = S1;
 end 
 S3:begin
  if(I==0)
   next_state = S0;
  else
   next_state = S4;
 end
 S4:begin
  if(I==0)
   next_state = S0;
  else
   next_state = S1;
 end
 default:next_state = S0;
 endcase
end

// combinational logic to determine the output
// of the Moore FSM, output only depends on current state
always @(state)
begin 
 case(state) 
 S0:   F = 0;
 S0: st_out = 3'b000;
 S1:   F = 0;
 S1: st_out = 3'b001;
 S2:  F = 0;
 S2: st_out = 3'b010;
 S3:  F = 0;
 S3: st_out = 3'b011;
 S4:  F = 1;
 S4: st_out = 3'b100;
 default:  F = 0;
 endcase
end 
endmodule
