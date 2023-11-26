# code29
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_arith.all;
  
ENTITY water_led is					--配置实体
	port(
		CLK,CLR : in std_logic;
		res : out std_logic_vector(7 downto 0)
	);
end ;

architecture func of water_led is					--配置结构体
signal state : integer range 11 downto 0 := 0;		--定义信号,全局量
begin
	process(CLR,CLK)			--监听时钟信号和清0信号
	begin
		if CLR = '1' then
			res <= "00000000";
			state <= 0;
		elsif CLK'event and CLK = '1' then 		--时钟变化且上升沿
			state <= state+1;			--每次触发全局量自增
			if state < 8 then
				res <= conv_std_logic_vector(2**state,8);
			elsif state = 8 then
				res <= "10101010";
			elsif state = 9 then
				res <= "01010101";
			elsif state = 10 then
				res <= "11111111";
			elsif state = 11 then
				res <= "00000000";
				state <= 0;		--从头开始
			end if;
		end if;
	end process;
end func;

