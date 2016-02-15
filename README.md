# hal-support

namespace UnrLab\Domain;

use JMS\Serializer\Annotation as JMS;
use UnrLab\Model\HalBuilder;
use UnrLab\Model\Serializable;

class Company implements Serializable {
    use HalBuilder;
    
    /**
     * @var integer
     * @JMS\Type("integer")
     */
    public $id;
    
    /**
     * @var string
     * @JMS\Type("string")
     */
    public $name;
    
    /**
     * @var string
     * @JMS\Type("string")
     */
    public $siren;
    
    /**
     * @var string
     * @JMS\Type("string")
     */
    public $address;
    
    /**
     * @var string
     * @JMS\Type("string")
     */
    public $zip;
    
    /**
     * @var string
     * @JMS\Type("string")
     */
    public $city;
    
    /**
     * @var Country
     * @JMS\Type("UnrLab\Domain\Country")
     */
    public $country;
    
    /**
     * @var array
     * @JMS\Exclude
     */
    public $users;
    
    /**
     * @var array
     * @JMS\Exclude
     */
    public $customers;

    /**
     * @var string
     * @JMS\Type("array")
     */
    protected $links;
    
    public function __construct() {
        $this->users = array();
        $this->customers = array();
    }
    
    public function __toString() {
        
        return $this->name;
    }

    /**
     * @JMS\PreSerialize
     */
    public function preSerialize()
    {
        $userIds     = array();
        $customerIds = array();
        if ($this->users->count() > 0) {
            foreach ($this->users as $user) {
                $userIds[] = $user->getId();
            }
            $this->links['users'] = $this->buildLinks($userIds, '/users/{id}', '{id}');
        }
        if ($this->customers->count() > 0) {
            foreach ($this->customers as $customer) {
                $customerIds[] = $customer->getId();
            }
            $this->links['customers'] = $this->buildLinks($customerIds, '/customers/{id}', '{id}');
        }
    }

    public function getLinks()
    {
        return $this->links;
    }
}
