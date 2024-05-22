import static org.mockito.Mockito.*;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;
import static org.springframework.test.web.servlet.setup.MockMvcBuilders.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

public class CostAndGrossControllerTest {

    private MockMvc mockMvc;
    private DealerService dealerService; // Mocked service
    private CostAndGrossDetailsRepository costAndGrossDetailsRepository; // Mocked repository

    @BeforeEach
    public void setup() {
        dealerService = mock(DealerService.class);
        costAndGrossDetailsRepository = mock(CostAndGrossDetailsRepository.class);
        CostAndGrossController controller = new CostAndGrossController(dealerService, costAndGrossDetailsRepository);
        mockMvc = MockMvcBuilders.standaloneSetup(controller).build();
    }

    @Test
    public void testDeleteForLead() throws Exception {
        int dealerId = 1; // Example dealer ID
        int leadSourceId = 10; // Example lead source ID

        mockMvc.perform(delete("/protected/1533244/{dealerId}/{leadSourceId}", dealerId, leadSourceId)
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isNoContent());

        // Optionally verify that interactions with mock objects occur as expected
        verify(costAndGrossDetailsRepository, times(1)).deleteByDealerIdAndLeadSourceId(dealerId, leadSourceId);
    }
}
