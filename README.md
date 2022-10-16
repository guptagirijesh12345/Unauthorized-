# Unauthorized-

// Controller------

    private $array=array('user_login','user_registration');
    public function __construct() 
    {
          
        parent::__construct();
        $this->load->model('ApiModel');
        $this->load->database(); 

        $this->id=$this->input->get_request_header('id');
        $this->session_key=$this->input->get_request_header('session_key');

        $this->method=$this->uri->segment(1); 
        if(in_array($this->method,$this->array)==false)
        {
          
         
         if(empty($this->id) || empty($this->session_key))
        {

          http_response_code(401);


           echo json_encode(array('status' =>   http_response_code(401) ,'message'=>'Unauthorized'),JSON_PRETTY_PRINT);
           http_response_code(401);
            exit;

        }
       
            $data=$this->ApiModel->check_session_key($this->id,$this->session_key);
            // print_r($data);die;
           if(!empty($data)) 
          {
            
           
            echo json_encode(array('status' => 401 ,'message'=>'Unauthorized'),JSON_PRETTY_PRINT);

          exit;
         }
      }
   }
 
 // model--------
 
    public function check_session_key($id,$session_key) //header
     {
             
      $query=$this->db->query("select * from user where id='$id' AND session_key='$session_key'")->row_array();
      if(!$query)
      {
        return 1;

      }
      else
      {
        return 0;
      }

     }
