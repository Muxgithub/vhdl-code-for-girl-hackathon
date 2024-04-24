# vhdl-code-for-girl-hackathon
vhdl code for the girl hackathon (on monte carlo simulation)

-- Example NoC Router component
entity noc_router is
  port (
    clk : in  std_logic;
    reset : in  std_logic;

    -- Input ports (from each direction)
    in_east  : in  packet_t;
    in_west  : in  packet_t;
    in_north : in  packet_t;
    in_south : in  packet_t;

    -- Output ports (to each direction)
    out_east : out packet_t;
    out_west : out packet_t;
    out_north: out packet_t;
    out_south: out packet_t;
  );
end entity noc_router;

architecture rtl of noc_router is
  -- ... (implementation logic for routing and arbitration)
begin
  -- ... (process for routing packets based on destination address)
    -- Logic to handle buffer size adjustments (can be external signals)
    if (buffer_size_ctrl_signal = '1') then
      -- Increase buffer size
    elsif (buffer_size_ctrl_signal = '0') then
      -- Decrease buffer size
    end if;

    -- Logic to implement weighted round-robin arbitration 
    -- Simplified pre-calculated round robin arbitration

signal cpu_count, io_count : natural range 0 to 1;
signal cpu_grant, io_grant : std_logic;

process(clk, reset)
begin
  if reset then
    cpu_count <= 0;
    io_count <= 0;
    cpu_grant <= '0';
    io_grant <= '0';
  elsif rising_edge(clk) then
    if in_east /= X"0000" then -- CPU request present
      cpu_grant <= '1';
      io_grant <= '0';
      if cpu_count = 1 then
        cpu_count <= 0;
        io_count <= 1;
      end if;
    elsif in_west /= X"0000" then -- IO request present
      cpu_grant <= '0';
      io_grant <= '1';
      if io_count = 1 then
        cpu_count <= 1;
        io_count <= 0;
      end if;
    else -- No requests, reset counters
      cpu_count <= 0;
      io_count <= 0;
      cpu_grant <= '0';
      io_grant <= '0';
    end if;
  end if;
end process;
