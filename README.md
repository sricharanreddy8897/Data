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

        DealerCostAndGrossResponse mockResponse = new DealerCostAndGrossResponse();
        ResponseEntity<DealerCostAndGrossResponse> responseEntity = ResponseEntity.ok(mockResponse);
        when(restTemplate.exchange(anyString(), eq(HttpMethod.GET), any(HttpEntity.class), eq(DealerCostAndGrossResponse.class)))
            .thenReturn(responseEntity);

        // Execute
        DealerCostAndGrossResponse response = dealerService.getDealerWithLeads(dealerId, month, year, offset, limit);

        // Verify
        assertNotNull(response);
        verify(restTemplate, times(1)).exchange(anyString(), eq(HttpMethod.GET), any(HttpEntity.class), eq(DealerCostAndGrossResponse.class));
    }

    @Test
    public void testGetDealerWithLeads_ClientError() {
        // Setup
        int dealerId = 1;
        String month = "January";
        int year = 2023;
        int offset = 0;
        int limit = 10;

        when(restTemplate.exchange(anyString(), eq(HttpMethod.GET), any(HttpEntity.class), eq(DealerCostAndGrossResponse.class)))
            .thenThrow(HttpClientErrorException.class);

        // Execute
        DealerCostAndGrossResponse response = dealerService.getDealerWithLeads(dealerId, month, year, offset, limit);

        // Verify
        assertNull(response);
        verify(restTemplate, times(1)).exchange(anyString(), eq(HttpMethod.GET), any(HttpEntity.class), eq(DealerCostAndGrossResponse.class));
    }

    @Test
    public void testGetDealerNotification_Success() {
        // Setup
        int dealerId = 1;
        LocalDate currentDate = LocalDate.now();
        int currentMonth = currentDate.getMonthValue();
        int currentYear = currentDate.getYear();

        DealerCostAndGrossResponse dealerCostAndGrossResponse = new DealerCostAndGrossResponse();
        dealerCostAndGrossResponse.setLeadSources(Collections.emptyList()); // Adjust based on your model

        ResponseEntity<DealerCostAndGrossResponse> responseEntity = ResponseEntity.ok(dealerCostAndGrossResponse);

        when(restTemplate.exchange(anyString(), eq(HttpMethod.GET), any(HttpEntity.class), eq(DealerCostAndGrossResponse.class)))
            .thenReturn(responseEntity);

        // Execute
        DealerNotificationResponse response = dealerService.getDealerNotification(dealerId);

        // Verify
        assertNotNull(response);
        verify(restTemplate, times(1)).exchange(anyString(), eq(HttpMethod.GET), any(HttpEntity.class), eq(DealerCostAndGrossResponse.class));
    }

    @Test
    public void testGetDealerNotification_ClientError() {
        // Setup
        int dealerId = 1;

        when(restTemplate.exchange(anyString(), eq(HttpMethod.GET), any(HttpEntity.class), eq(DealerCostAndGrossResponse.class)))
            .thenThrow(HttpClientErrorException.class);

        // Execute
        DealerNotificationResponse response = dealerService.getDealerNotification(dealerId);

        // Verify
        assertNotNull(response);
        assertFalse(response.isDealerGrossFound());
        verify(restTemplate, times(1)).exchange(anyString(), eq(HttpMethod.GET), any(HttpEntity.class), eq(DealerCostAndGrossResponse.class));
    }
}
