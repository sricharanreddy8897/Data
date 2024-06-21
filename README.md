import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpMethod;
import org.springframework.http.ResponseEntity;
import org.springframework.http.HttpStatus;
import org.springframework.web.client.HttpClientErrorException;
import org.springframework.web.client.RestTemplate;

import java.time.LocalDate;
import java.util.Collections;

public class DealerServiceImplTest {

    @InjectMocks
    private DealerServiceImpl dealerService;

    @Mock
    private RestTemplate restTemplate;

    @BeforeEach
    public void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    public void testGetDealerWithLeads_Success() {
        // Setup
        int dealerId = 1;
        String month = "January";
        int year = 2023;
        int offset = 0;
        int limit = 10;

        DealerCostAndGrossResponse dealerResponse = new DealerCostAndGrossResponse();
        ResponseEntity<DealerCostAndGrossResponse> responseEntity = ResponseEntity.ok(dealerResponse);
        when(restTemplate.exchange(
                eq("http://localhost:8089/costandgrossget?month=January&year=2023&offset=0&limit=10"),
                eq(HttpMethod.GET),
                any(HttpEntity.class),
                eq(DealerCostAndGrossResponse.class)
        )).thenReturn(responseEntity);

        // Execute
        DealerCostAndGrossResponse response = dealerService.getDealerWithLeads(dealerId, month, year, offset, limit);

        // Verify
        assertNotNull(response);
        verify(restTemplate, times(1)).exchange(
                eq("http://localhost:8089/costandgrossget?month=January&year=2023&offset=0&limit=10"),
                eq(HttpMethod.GET),
                any(HttpEntity.class),
                eq(DealerCostAndGrossResponse.class)
        );
    }

    @Test
    public void testGetDealerNotification_Success() {
        // Setup
        int dealerId = 1;

        DealerCostAndGrossResponse dealerResponse = new DealerCostAndGrossResponse();
        dealerResponse.setLeadSources(Collections.emptyList()); // Adjust based on your model

        ResponseEntity<DealerCostAndGrossResponse> responseEntity = ResponseEntity.ok(dealerResponse);

        when(restTemplate.exchange(
                eq("http://localhost:8089/costandgrossget?month=June&year=2023&offset=0&limit=10"),
                eq(HttpMethod.GET),
                any(HttpEntity.class),
                eq(DealerCostAndGrossResponse.class)
        )).thenReturn(responseEntity);

        // Execute
        DealerNotificationResponse response = dealerService.getDealerNotification(dealerId);

        // Verify
        assertNotNull(response);
        verify(restTemplate, times(1)).exchange(
                eq("http://localhost:8089/costandgrossget?month=June&year=2023&offset=0&limit=10"),
                eq(HttpMethod.GET),
                any(HttpEntity.class),
                eq(DealerCostAndGrossResponse.class)
        );
    }
}
