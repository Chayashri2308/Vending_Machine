# Vending_Machine
# A simple Vending Machine design for only one product using Verilog. The user can enter three different currency notes and based on the price of the product, the machine dispenses the product and gives the change. 

`timescale 1ns / 1ps

module vending_machine(
    input clk,
    input reset,
    input [1:0] currency, // Currency type: 2'b00 = 1 unit, 2'b01 = 2 units, 2'b10 = 5 units
    input buy,            // Button to buy the product
    output reg dispense,   // Dispense the product
    output reg [3:0] change // Change to be returned
);

    // Parameters
    parameter PRODUCT_PRICE = 5; // Price of the product in units
    
    // Registers
    reg [3:0] total_amount; // Total amount inserted

    // Process currency inputs
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            total_amount <= 0;
            dispense <= 0;
            change <= 0;
        end else if (buy) begin
            // Check if sufficient funds are available to buy the product
            if (total_amount >= PRODUCT_PRICE) begin
                dispense <= 1;
                change <= total_amount - PRODUCT_PRICE; // Calculate the change
                total_amount <= 0; // Reset the amount after purchase
            end else begin
                dispense <= 0; // Insufficient funds
                change <= 0;
            end
        end else begin
            dispense <= 0;
            change <= 0;
            // Update total amount based on currency type inserted
            case (currency)
                2'b00: total_amount <= total_amount + 1; // 1 unit
                2'b01: total_amount <= total_amount + 2; // 2 units
                2'b10: total_amount <= total_amount + 5; // 5 units
                default: total_amount <= total_amount;   // No change
            endcase
        end
    end
endmodule
