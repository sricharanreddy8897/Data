
import static org.mockito.ArgumentMatchers.anyInt;
import static org.mockito.Mockito.when;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertTrue;

import java.util.Collections;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
public class DealerNotificationServiceTest {

    @InjectMocks
    private DealerNotificationService dealerNotificationService;

    @Mock
    private DealerCostAndGrossResponse dealerCostAndGrossResponse;

    @BeforeEach
    public void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    public void testGetDealerNotification() {
        // Setup mock response
        when(dealerCostAndGrossResponse.getLeadSources()).thenReturn(Collections.emptyList());
        
        // Mock the service call that fetches the dealer cost and gross response
        when(dealerNotificationService.getDealerWithLeads(anyInt(), anyString(), anyInt(), anyInt(), anyInt()))
            .thenReturn(dealerCostAndGrossResponse);

        // Call the service method
        DealerNotificationResponse response = dealerNotificationService.getDealerNotification(1);

        // Validate the response
        assertNotNull(response);
        assertTrue(response.isGrossRequired());
    }
}
