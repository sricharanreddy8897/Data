import static org.mockito.Mockito.*;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;
import static org.springframework.test.web.servlet.setup.MockMvcBuilders.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;

@WebMvcTest(CostAndGrossController.class)
public class CostAndGrossControllerTest {

    private MockMvc mockMvc;

    @MockBean
    private DealerService dealerService; // Mocked service

    @InjectMocks
    private CostAndGrossController controller;

    @BeforeEach
    public void setup() {
        mockMvc = MockMvcBuilders.standaloneSetup(controller).build();
    }

    @Test
    public void testGetCostAndGrossForLeads_NoDealerFound() throws Exception {
        int dealerId = 1;
        String month = "June";
        int year = 2022;
        int offset = 0;
        int limit = 10;

        // Setup the mock to return null, simulating no dealer found
        when(dealerService.getDealerWithLeads(dealerId, month, year, offset, limit)).thenReturn(null);

        mockMvc.perform(get("/{dealerId}/marketing/leads", dealerId)
                .param("month", month)
                .param("year", String.valueOf(year))
                .param("offset", String.valueOf(offset))
                .param("limit", String.valueOf(limit))
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isNotFound())
                .andExpect(content().string(containsString("Dealer with ID " + dealerId + " not found or no leads available for the specified period.")));
    }
}
