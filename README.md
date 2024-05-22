 import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(CostAndGrossController.class)
public class CostAndGrossControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private DealerService dealerService;

    @MockBean
    private CostAndGrossDetailsRepository costAndGrossDetailsRepository;

    @Test
    public void testDeleteForLead() throws Exception {
        mockMvc.perform(delete("/protected/1533244")
                .param("dealerId", "1")
                .param("LeadSourceId", "1")
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isNoContent());
    }

    @Test
    public void testGetForLead() throws Exception {
        mockMvc.perform(get("/1/marketing/leads/1")
                .param("month", "2024-05")
                .param("year", "2024")
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isNoContent());
    }

    @Test
    public void testSaveCostAndGrossForLeads() throws Exception {
        DealerCostAndGross dealerRequest = new DealerCostAndGross();
        when(dealerService.processDealerData(1, dealerRequest)).thenReturn(ResponseEntity.status(201).body("Created"));

        mockMvc.perform(post("/1/marketing/leads")
                .content("{\"dealerId\":1,\"leadSources\":[],\"pagination\":{}}")
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isCreated())
                .andExpect(content().string("Created"));
    }

    @Test
    public void testGetCostAndGrossForLeads() throws Exception {
        DealerCostAndGrossResponse dealerResponse = new DealerCostAndGrossResponse();
        when(dealerService.getDealerWithLeads(1, "2024-05", 2024, 0, 10)).thenReturn(dealerResponse);

        mockMvc.perform(get("/1/marketing/leads")
                .param("month", "2024-05")
                .param("year", "2024")
                .param("offset", "0")
                .param("limit", "10")
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk());
    }
}
