import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;
import java.util.stream.Collectors;

@Service
public class DealerService {

    private final CostAndGrossDetailsRepository costAndGrossDetailsRepository;
    private final LeadMapper leadMapper;
    private final DealerMapper dealerMapper;

    @Autowired
    public DealerService(CostAndGrossDetailsRepository costAndGrossDetailsRepository, LeadMapper leadMapper, DealerMapper dealerMapper) {
        this.costAndGrossDetailsRepository = costAndGrossDetailsRepository;
        this.leadMapper = leadMapper;
        this.dealerMapper = dealerMapper;
    }

    @Transactional
    public String processDealerData(Dealer dealer) {
        try {
            // Save to database
            costAndGrossDetailsRepository.saveAll(dealer.getLeads());

            return "Data inserted successfully";
        } catch (Exception e) {
            // Handle exception, log error, etc.
            return "Failed to insert data: " + e.getMessage();
        }
    }
}
