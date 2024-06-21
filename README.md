
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.http.ResponseEntity;
import org.springframework.web.client.HttpClientErrorException;
import org.springframework.web.client.RestTemplate;

import java.time.LocalDate;

public class DealerServiceImplTest {

    @InjectMocks
    private DealerServiceImpl dealerService;

    @Mock
    private RestTemplate restTemplate;

    @BeforeEach
    public void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    // Existing tests...

    @Test
    public void testGetDealerNotification_Success() {
        // Setup
        int dealerId = 1;
        LocalDate currentDate = LocalDate.now();
        int currentMonth = currentDate.getMonthValue();
        int currentYear = currentDate.getYear();
        
        DealerNotificationResponse mockResponse = new DealerNotificationResponse();
        DealerCostAndGrossResponse dealerCostAndGrossResponse = new DealerCostAndGrossResponse();
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




import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;

public class CostAndGrossControllerTest {

    @InjectMocks
    private CostAndGrossController costAndGrossController;

    @Mock
    private DealerService dealerService;

    @BeforeEach
    public void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    // Existing tests...

    @Test
    public void testGetDealerNotification_Success() {
        // Setup
        int dealerId = 1;
        
        DealerNotificationResponse mockResponse = new DealerNotificationResponse();
        when(dealerService.getDealerNotification(dealerId)).thenReturn(mockResponse);
        
        // Execute
        ResponseEntity<DealerNotificationResponse> response = costAndGrossController.getDealerNotification(dealerId);
        
        // Verify
        assertNotNull(response);
        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertEquals(mockResponse, response.getBody());
        verify(dealerService, times(1)).getDealerNotification(dealerId);
    }

    @Test
    public void testGetDealerNotification_DealerNotFound() {
        // Setup
        int dealerId = 1;
        
        when(dealerService.getDealerNotification(dealerId)).thenReturn(null);
        
        // Execute
        ResponseEntity<DealerNotificationResponse> response = costAndGrossController.getDealerNotification(dealerId);
        
        // Verify
        assertNotNull(response);
        assertEquals(HttpStatus.NOT_FOUND, response.getStatusCode());
        assertNull(response.getBody());
        verify(dealerService, times(1)).getDealerNotification(dealerId);
    }
}
